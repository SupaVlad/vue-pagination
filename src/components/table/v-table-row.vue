<template>
	<div class="card">
		<img :src="imgPreUrl + row_data.image[0].url" />
		<h4>{{ row_data.title }}</h4>
		<div class="card-text">Производитель: {{ row_data.manufacturer.title }}</div>
		<div class="card-text" v-if="row_data.material">Материал: {{ row_data.material.title }}</div>
		<div class="card-text" v-if="row_data.manufacturer">Страна производитель: {{ row_data.manufacturer.title }}</div>
		<div>
			<div class="card-text-with" v-if="row_data.special_price">
				<span class="card-text-sell">{{ row_data.price }} р. </span>
				<span class="card-text-old"> {{ row_data.special_price }} р.</span>
			</div>
			<div class="card-text-no" v-else>
				<span> {{ row_data.price }} р.</span>
			</div>
		</div>
	</div>
</template>

<script>
  export default {
    name: "",
    data: () => ({

    }),
    // components: {},
    methods: {},
    computed: {
      imgPreUrl : function() {
        return "http://95.217.16.207:1337";
      },
    },
    watch: {},
    props: {
      row_data: {
        type: Object,
        default: () => {
            return {}
        }
      }
    },
  }
</script>

<style>
	.card{
		width: 302px;
		margin-right: 16px;
		text-align: center;
		margin-bottom: 43px;
	}
	.card img{
		width: 302px;
		height: 302px;
		object-fit: cover;
	}
	h4{
		margin-top: 20px;
		margin-bottom: 16px;
		font-style: normal;
		font-weight: 500;
		font-size: 16px;
		line-height: 20px;
		text-align: center;
		color: #222222;
	}
	.card-text{
		font-style: normal;
		font-weight: 500;
		font-size: 13px;
		line-height: 16px;
		text-align: center;
		color: #B9B9B9;
		margin-bottom: 3px;
	}
	.card-text-sell{
		font-style: normal;
		font-weight: bold;
		font-size: 16px;
		line-height: 20px;
		text-align: center;
		color: #222222;
	}
	.card-text-old{
		font-style: normal;
		font-weight: bold;
		font-size: 16px;
		line-height: 20px;
		text-align: center;
		color: #B9B9B9;
		text-decoration: line-through;
	}
	.card-text-no span{
		font-style: normal;
		font-weight: bold;
		font-size: 16px;
		line-height: 20px;
		text-align: center;
		color: #222222;
	}
</style>
