<template>
    <div>
        
        <input type="text" v-model="url" placeholder="Enter URL" class="form-control text-center">
        
        <div class="text-center">

            <button v-if="!(loadingMeta || loadingAudio || loadingSumma)" class="btn btn-primary btn-lg mt-4" @click="fetchMetaData"><i class="fas fa-align-left"></i> Summify!</button>
            <button v-else class="btn btn-warning btn-lg mt-4" disabled><i class="fas fa-align-left"></i> <em>Summifying...</em></button>
            
        </div>

        <h4 v-if="offline">The endpoint client is offline! Please turn it on</h4>
        <!-- <hr/> -->

        <div v-if="loadingMeta || loadingAudio || loadingSumma">
            
            <div class="text-center">
                <!-- <Spinner/> -->
                <p>Working on it!</p>
            </div>

            <div v-if="meta && meta.length">
                <Pbar :expectedSeconds="meta.length * 0.6" :completed="loadingSumma"/>
            </div>
        </div>
        <div class="timeline-ui">

            <div class="summa-ui card card-body mt-3" v-if="summaryText">
                <h1 class="text-primary text-thin"><i class="fas fa-compress"></i>  Summified!</h1>

                <ul class="mt-3">
                    <li v-for="(item, index) in textInPoints" :key="index">{{ item }}</li>
                </ul>
            </div>

            <div class="audio-ui card card-body mt-4" v-if="audioUrl">
                <div v-if="audioUrl" class="p-2">
                    <h4><i class="fas fa-file-audio"></i> Here's the audio file:</h4>
                    <audio controls class="mt-3" autoplay>
                        <source :src="audioUrl" type="audio/mpeg">
                        Your browser does not support the audio element.
                    </audio> 
                </div>
            </div>

            <div class="meta-ui card card-body mt-4" v-if="meta">
                <h5><i class="fas fa-video"></i> About your video</h5>

                <div class="row mt-4">
                    <div class="col-12">
                        <span class="text-primary">Title:</span><h3 class="mb-5">{{ meta.title }}</h3>
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
                <p class="lead mt-4">It might take about <span class="text-bold">{{ Math.round(meta.length * 0.6) }}</span> seconds to summarize this video, that's saving 40% of your time.</p>
            </div>

          
            
        </div>

    </div>
</template>
<script>
import axios from 'axios';
import Pbar from '@/components/PBar.vue'
import Spinner from '@/components/Spinner.vue'
export default {
    components:{
        Pbar, Spinner
    },
    data(){
        return{
            offline:false,
            loadingMeta:false,
            loadingAudio:false,
            loadingSumma:false,
            meta:null,
            audioUrl:null,
            rawAudioUrl:null,
            summaryText:null,
            url:'',
            endpointUrlBase:'http://127.0.0.1:5000/youtube',
            urlBase:'http://127.0.0.1:5000'
        }
    },
    computed:{
        textInPoints(){
            if(this.summaryText){
                return this.summarizeText(this.summaryText)
            }else{
                return []
            }
        }
    },  
    methods:{
        summarizeText(text) {
            const summaryPoints = [];

            // Split the text into sentences
            const sentences = text.split('. ');

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
        fetchMetaData(){     
            this.loadingAudio = true
            this.loadingSumma = true       
            this.loadingMeta = true;
            this.meta = null
            this.audioUrl = null
            this.summaryText = null
            this.rawAudioUrl = null
    
            // Make a POST request to the Flask API for video metadata
            axios.post(this.endpointUrlBase + '/video_meta', { url: this.url })
            .then(response => {
                const dat = response.data.body

                this.meta = dat.metadata
                this.loadingMeta = false;

                this.fetchAudioFile()
            }).catch((err)=>{
                this.offline = true
            })
        },
        fetchAudioFile(){            
            this.loadingAudio = true;
    
            // Make a POST request to the Flask API for video metadata
            axios.post(this.endpointUrlBase + '/audio_file', { url: this.url })
            .then(response => {
                const dat = response.data.body

                this.rawAudioUrl = this.encodeFilePathForAudioURL(dat.audio_path)

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
        fetchTextFromAudio(audioPath){            
            this.loadingSumma = true;
    
            console.debug('Sending audioPath:' + audioPath)

            // Make a POST request to the Flask API for video metadata
            axios.post(this.endpointUrlBase + '/summarize', { url: audioPath })
            .then(response => {
                const dat = response.data.body

                this.summaryText = dat.text
                console.debug('Text response: ' + JSON.stringify(response.data))
                this.loadingSumma = false;
            });
        }
    }
}
</script>
<style>
    
</style>