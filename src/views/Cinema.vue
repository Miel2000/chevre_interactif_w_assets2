<template>
    <div>

        <div class="video_and_cta">

            <ComponentAudio 
                :audio-infos="audioInfos"
            />

            <ComponentCallToAction/>
            <!-- v-on:playAfterCta="playAfterCta"  -->

        </div>


        <div v-if="computedChoices.length > 0" >
            <div
                v-for="choice in computedChoices" 
                :key="choice.id">

                <ComponentOneChoice 
                    :choice-infos="choice"
                    @a-choice-have-been-acted="choiceActedHandler"
                />

            </div>
        </div>

        <div v-if="computedAudios.length > 0" >
            <div v-for="audio in computedAudios"
                :key="audio.id">

                <!-- <ComponentAudio @an-action-is-sent="computedAudios"  :audio-infos="audio" />  -->
            </div>
        </div>

    </div>
</template>


<!-- ° ° ° ° ° ° ° ° ° L O G I C ° ° ° ° ° ° ° ° ° -->
<!-- ° ° ° ° ° ° ° ° ° L O G I C ° ° ° ° ° ° ° ° ° -->
<script>

    import { bus } from '@/main'
 
    import ComponentAudio from '@/components/ComponentAudio';
    // import ComponentAudio from '@/components/ComponentAudio';
    import ComponentCallToAction from '@/components/ComponentCallToAction';
    import ComponentOneChoice from '@/components/ComponentOneChoice';

    import storyMap from '@/storyMap.js';


	export default {

		components: {
            ComponentAudio,
            ComponentCallToAction,
            ComponentOneChoice,
            // ComponentAudio
        },

        props: {
            audioId: {
                type: String,
                default: "weed"
            }
        },
        
        data() {
            return {
                audioInfos: storyMap.videos[this.audioId],
                choices: [],
                audios: [],
            }
        },

        computed: {

            computedChoices(){
                return this.choices;
            },
            computedAudios(){
                return this.audios;
            }

        },

        mounted() {
            // 
            console.log("cinema mounted");
            console.log("cinema mounted");
            console.log("cinema mounted");
            console.log("cinema mounted");
            console.log("cinema mounted");

            // new emit 
            bus.$on("an-action-is-sent", this.actionHandler);  
        },

        methods: {

            actionHandler(actionInfos){

                console.log("le looooggg : ", actionInfos);
                console.log("le looooggg du bus: ", bus.testArr);
                bus.testArr.push("ponnnnnggg");
                bus.$emit("say-pong", "wesh");

                // console.log("hey, je suis le parent, et jai recu un event choice avec : ", actionInfos);
            
                switch (actionInfos.type) {
                    case "choice":
                        console.log('dans le switch CHOICE : ',actionInfos)
                        this.choices.push(actionInfos);
                        break;

                    case "sound":
                        console.log('dans le switch SOUND : ',actionInfos);
                       
                        this.audios.push(actionInfos);
                        break;
                
                    default:
                        break;
                }

            },

            choiceActedHandler(choice) {

                this.audioInfos = storyMap.videos[choice];

            }

         
            
        }

    }
    
</script>

<style lang="scss">

    .scene-container {
        width: 70%;
        margin: 0 auto;
        display: block;
    }

    .video_and_cta {

        width: 100%;
        height: 100%;

        display: flex;
        justify-content: center;
        align-items: center;
    }

</style>