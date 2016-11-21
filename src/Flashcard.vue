<template lang="jade">
.flashcard-container
	.flashcard-prompt.text-center
		h1 What is this rank?
	.flashcard(:class="{revealed: revealed}")
		.flashcard-front(@click="reveal")
			img.center-block(:src="cardData.image")
		.flashcard-back
			p.lead.answer.text-center(v-if="revealed") {{cardData.answer}}
	button.btn-block.btn.btn-lg.btn-primary(v-if="!revealed" @click="reveal") Reveal Answer
	button.btn-block.btn.btn-lg.btn-primary(v-if="revealed" @click="nextButtonClicked") Next Card
</template>

<script>
export default {
	name: 'flashcard',
	props: ['cardData'],
	data() {
		return {
			revealed: false
		}
	},
	watch: {
		cardData (newValue, oldValue) {
			// When the card data updates, unflip
			this.revealed = false;
		}
	},
	methods: {
		reveal () {
			if(this.revealed)
				return;
			this.revealed = true;
			this.$emit('answerRevealed');
		},
		nextButtonClicked () {
			this.$emit('nextButtonClicked');
		}
	}
}
</script>

<style lang="sass">
.flashcard-container {
	.flashcard-prompt {
		margin: 1em 0;
	}
	.flashcard {
	    .flashcard-front {
	    	img {
	    		width: 100%;
	    		max-width: 320px;
	    		max-height: 400px;
	    	}
	    }
	    .flashcard-back {
	    	.answer {
	    		margin: 20px 0;
	    	}
	    }
	}
}
</style>