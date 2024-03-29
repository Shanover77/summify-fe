<template>
    <div>

        <div style="width: 100%;align-items: center;display: flex;justify-content: center;">
            <input type="text" v-model="url" placeholder="Enter URL" class="form-control text-center txb">
        </div>

        <div class="text-center">

            <button v-if="!(loadingMeta || loadingAudio || loadingSumma)" class="btn btn-primary btn-lg mt-4"
                @click="fetchMetaData"><i class="fas fa-align-left"></i> Summify!</button>
            <button v-else class="btn btn-warning btn-lg mt-4" disabled><i class="fas fa-align-left"></i>
                <em>Summifying...</em></button>

        </div>

        <h4 v-if="offline">The endpoint client is offline! Please turn it on</h4>
        <!-- <hr/> -->

        <div v-if="loadingMeta || loadingAudio || loadingSumma">

            <div class="text-center">
                <!-- <Spinner/> -->
                <p>Working on it!</p>
            </div>

            <div v-if="meta && meta.length">
                <Pbar :expectedSeconds="meta.length * 0.6" :completed="loadingSumma" />
            </div>
        </div>
        <div class="timeline-ui">

            <div class="summa-ui card card-body mt-3" v-if="formattedContent">
                <h3 class="text-primary text-thin"><i class="fas fa-compress"></i> Short Summary</h3>
                <p>{{ formattedContent }}</p>
            </div>

            <div class="summa-ui card card-body mt-3" v-if="summaryText">
                <h3 class="text-primary text-thin"><i class="fas fa-compress"></i> Long Summary</h3>

                <ul class="mt-3">
                    <li v-for="(item, index) in textInPoints" :key="index">{{ item }}</li>
                </ul>

                <div class="p-3 mb-3">
                    <div v-if="wordCloudFilePath" class="p-2">
                        <h4><i class="fas fa-file-audio"></i> Word Cloud:</h4>
                        <img :src="wordCloudFilePath" style="max-height: 500px;" />
                    </div>
                </div>

                <div v-if="textFilePath" class="p-2">
                    <h4><i class="fas fa-file-audio"></i> Text file:</h4>
                    <a :href="textFilePath" target="_blank" download="true"><i class="fas fa-cloud-download-alt"></i> Download Transcript</a>
                </div>

            </div>

            <div class="audio-ui card card-body mt-4" v-if="audioUrl">
                <div v-if="audioUrl" class="p-2">
                    <h4><i class="fas fa-file-audio"></i> Here's the audio file:</h4>
                    <audio controls class="mt-3" autoplay>
                        <source :src="audioUrl" type="audio/mpeg">
                        Your browser does not support the audio element.
                    </audio>

                    <br/>
                    <br/>
                    <a :href="audioUrl" target="_blank" download><i class="fas fa-cloud-download-alt"></i> MP3 Audio File</a>
                </div>
            </div>

            <div class="meta-ui card card-body mt-4" v-if="meta">
                <h5><i class="fas fa-video"></i> Information about video</h5>

                <div class="row mt-4">
                    <div class="col-12">
                        <span class="text-primary">Title:</span>
                        <h3 class="mb-5">{{ meta.title }}</h3>
                    </div>
                    <div class="col-4">
                        <h5 class="text-primary">Length</h5>
                        <h4>{{ meta.length }}s</h4>
                    </div>
                    <div class="col-4">
                        <h5 class="text-primary">Author</h5>
                        <h4>{{ meta.author }}</h4>
                    </div>
                    <div class="col-4">
                        <h5 class="text-primary">Published on</h5>
                        <h4>{{ meta.publish_date.split('00:00:00')[0] }}</h4>
                    </div>
                </div>
                <p class="lead mt-4">It might take about <span class="text-bold">{{ Math.round(meta.length * 0.6) }}</span>
                    seconds to summarize this video, that's saving 40% of your time.</p>
            </div>



        </div>

    </div>
</template>
<script>
import axios from 'axios';
import Pbar from '@/components/PBar.vue'
import Spinner from '@/components/Spinner.vue'
export default {
    components: {
        Pbar, Spinner
    },
    data() {
        return {
            offline: false,
            loadingMeta: false,
            loadingAudio: false,
            loadingSumma: false,
            meta: null,
            audioUrl: null,
            rawAudioUrl: null,
            summaryText: null,
            textFilePath: null,
            wordCloudFilePath: null,
            url: '',
            endpointUrlBase: 'http://127.0.0.1:5000/',
            urlBase: 'http://127.0.0.1:5000',
            formattedContent:null
        }
    },
    computed: {
        textInPoints() {
            if (this.summaryText) {
                return this.summarizeText(this.summaryText)
            } else {
                return []
            }
        }
    },
    methods: {
        inferSummarization(text){
            console.debug('Summarizing.............................................')
            const API_URL = 'https://api-inference.huggingface.co/models/facebook/bart-large-cnn';
            const API_KEY = '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';
            axios.post(API_URL, {
                inputs: text
            }, {
                headers:{
                    Authorization: `Bearer ${API_KEY}`
                }
            }).then((response)=>{
                const data = response.data
                console.debug('HF Resp: ' + typeof data)
                console.debug('HF Resp: ' + JSON.stringify(data))
                if (data.length > 0) {
                    this.formattedContent = data[0].summary_text
                }
            })
        },
        summarizeText(text) {
            const summaryPoints = [];

            // Split the text into sentences
            const sentences = text.split('. ');

            this.inferSummarization(text)

            // Extract key information from each sentence
            sentences.forEach((sentence) => {
                const trimmedSentence = sentence.trim();

                // Check if the sentence contains relevant information
                if (trimmedSentence.length > 0 && !trimmedSentence.endsWith(':')) {
                    // Add the sentence to the summary points array
                    summaryPoints.push(trimmedSentence);
                }
            });

            return summaryPoints;
        },
        encodeFilePathForAudioURL(filePath) {
            const encodedPath = filePath.replace(/\\/g, "/");
            return encodeURI(encodedPath);
        },
        processFilePath(path, type) {
            console.debug('Got path:' + path + '> Type:' + type)
            path = this.encodeFilePathForAudioURL(path)
            const startIndex = path.indexOf("/static");

            // Extract the desired part of the URL
            const convertedPath = path.substring(startIndex);

            const fin = this.urlBase + convertedPath
            console.debug('final:' + fin)
            return fin
        },
        fetchMetaData() {
            this.loadingAudio = true
            this.loadingSumma = true
            this.loadingMeta = true;
            this.meta = null
            this.audioUrl = null
            this.summaryText = null
            this.rawAudioUrl = null

            // Make a POST request to the Flask API for video metadata
            axios.get(this.endpointUrlBase + 'video/metadata?url=' + this.url)
                .then(response => {
                    const dat = response.data.body

                    this.meta = dat.metadata
                    this.loadingMeta = false;

                    this.fetchAudioFile()
                }).catch((err) => {
                    this.offline = true
                })
        },
        fetchAudioFile() {
            this.loadingAudio = true;

            // Make a POST request to the Flask API for video metadata
            axios.post(this.endpointUrlBase + 'video/download', { url: this.url })
                .then(response => {
                    const dat = response.data.body

                    this.rawAudioUrl = this.encodeFilePathForAudioURL(dat.file_path)

                    const rurl = this.rawAudioUrl

                    const startIndex = rurl.indexOf("/static");

                    // Extract the desired part of the URL
                    const convertedPath = rurl.substring(startIndex);

                    console.log(convertedPath);

                    this.audioUrl = this.urlBase + convertedPath

                    this.loadingAudio = false;

                    this.fetchTextFromAudio(this.audioUrl)
                });
        },
        fetchTextFromAudio(audioPath) {
            this.loadingSumma = true;

            console.debug('Sending audioPath:' + audioPath)

            // Make a POST request to the Flask API for video metadata
            axios.post(this.endpointUrlBase + 'audio/convert', { file_path: audioPath })
                .then(response => {
                    const dat = response.data.body

                    this.summaryText = dat.text
                    this.textFilePath = this.processFilePath(dat.text_file_path, 'text_files')
                    this.wordCloudFilePath = this.processFilePath(dat.word_cloud_path, 'word_clouds')
                    console.debug('Text response: ' + JSON.stringify(dat))
                    this.loadingSumma = false;
                });
        }
    }
}
</script>
<style></style>