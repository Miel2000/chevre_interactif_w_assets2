<!-- Principe : -->
<!-- Ce component fait beaucoup de choses : -->
<!-- _ il surveille le currentTime de la video pour déclencher des vérifications selon celui-ci -->
<!-- __ quand il doit déclencher des choses, c'est souvent : -->
<!-- ___ mettre à jour le $store.state (les "actualChoices actualCallToActions" etc..) -->
<!-- ___ parfois des .play() ou des .pause() ou des $store.$emit() selon les spécificités des différents éléments d'intéractions -->

<!-- ° ° ° ° ° ° ° ° ° T E M P L A T E ° ° ° ° ° ° ° ° ° -->
<!-- ° ° ° ° ° ° ° ° ° T E M P L A T E ° ° ° ° ° ° ° ° ° -->

<template>

	<div>

		<audio 
			class="audio-player"
			:class="{ 
				cohabitationCta: this.$store.state.actualCallToActions.length !== 0,
				isInteractive: this.$store.state.playerIsInteractive
			}"
			:src="'/assets/mp3/' + audioInfos.self.url"
			controls 
			@timeupdate="onTimeUpdate" 
		>
		</audio>

		<button
			@click="testHandler"
		>
			heyhey
		</button>

	</div>


</template>

<!-- ° ° ° ° ° ° ° ° ° L O G I C ° ° ° ° ° ° ° ° ° -->
<!-- ° ° ° ° ° ° ° ° ° L O G I C ° ° ° ° ° ° ° ° ° -->

<script>
	import { bus } from '@/main'

	export default {


		props: {
			audioInfos: {
				type: Object,
				required: true
			}
		},

		data() {
			return {};
		},

		mounted() {

			this.isPaused = false;

			this.alreadySent = [];

			bus.$on("say-pong", () => { 
				console.log("PONNG ", bus.testArr);
				console.log("PONNG ", bus.currentAudioPlaying);
			});


		},

		methods: {

			onTimeUpdate( event ) {
					
					if(this.audioInfos.timedActions){

						this.audioInfos.timedActions.forEach( actionInfos => {

						if ( this.alreadySent.indexOf(actionInfos.id) === -1) {

							this.compareTimeCodes(event.target.currentTime, actionInfos.at, actionInfos);

						}

					});
				
				}
			},

			testHandler(){

				bus.testArr = ["fock"];

				bus.currentAudioPlaying = this.$el.querySelector("audio");
				// new emit
				bus.$emit("an-action-is-sent", "coucou");

			},

				

			compareTimeCodes(currentTimeVideo, timeCodeToTrigger, action) {
				
					
				// console.log('ALL ACTIONS  : ',action);

					if(action.type){

						if ( currentTimeVideo >= timeCodeToTrigger ) {

						console.log("weh on emit l'action");
					
				
						//  old emit
						// this.$emit("an-action-is-sent", action);

						bus.testArr = ["fock"];
						// new emit
						bus.$emit("an-action-is-sent", "coucou");

				

						this.alreadySent.push(action.id);
						
					}

				
				}
			}

		}
	}
	




</script>


<!-- ° ° ° ° ° ° ° ° ° S T Y L E ° ° ° ° ° ° ° ° ° -->
<!-- ° ° ° ° ° ° ° ° ° S T Y L E ° ° ° ° ° ° ° ° ° -->

<style scoped lang="scss">

	.video-player {

		width: 100%;

		// de base, le player n'est pas interactif
		pointer-events: none;
		border: solid 15px red;


		transition:
			border .7s,
			width .7s;

		&.cohabitationCta {
			width: 80%;
			margin-right: 50px;
		}

		&.isInteractive {
			pointer-events: initial;
			border: solid 15px green;
		}

	}

</style>
