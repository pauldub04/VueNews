<template>
<v-card
    class="mx-auto card"
    max-width="400"
>
    <v-img
    class="white--text align-end image"
    height="200px"
    :src="imgSrc"
    >
    <v-card-title class="title">{{title}}</v-card-title>
    </v-img>

    <v-card-subtitle class="pb-0">{{author}}</v-card-subtitle>

    <v-card-text class="text--primary">

    <div>{{desc}}</div>
    </v-card-text>

</v-card>
</template>

<script>
export default {
	name: 'ArticleTile',
	props: ['title', 'author', 'desc', 'img', 'url'],
	data() {
		return {
		}
	},
    computed: {
        imgSrc() {
            if (this.img !== null)
                return this.img;
            else
                return 'http://denrakaev.com/wp-content/uploads/2015/03/no-image-800x511.png';
        }
    }
}
</script>

<style>
.card {
    margin-bottom: 30px;
}

.title:hover {
    color:  #ffaa00;
    transition: 0.3s;
}

</style>