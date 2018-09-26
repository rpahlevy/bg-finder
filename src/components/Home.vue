<template>
  <div class="text-center">
    <h1>Board Game Finder</h1>
	<form>
		<input id="keyword" type="text" v-model="keyword">
	</form>
	<p v-if="keyword.length === 0" class="text-muted f-small">Start typing to search your favorite Board Games</p>
	<ul class="text-left">
		<li v-for="b in boardgames">
			<img :src="b.thumbnail[0].$.value">
			{{b.name[0].$.value}}
		</li>
	</ul>
	<div v-if="keyword.length > 0 && boardgames.length === 0" class="text-danger">No boardgames found with keyword <strong>{{keyword}}</strong></div>
  </div>
</template>

<script>
var parseString = require('xml2js').parseString;

export default {
  name: 'Home',
  data () {
    return {
		keyword: "",
		boardgames: []
    }
  },
  methods: {
	parseXML: function(xml) {
		let res = "";
		parseString(xml, (err,result) => {
			if (err) {
				console.log('Error parsing: '+ err);
			} else {
				res = result;
			}
		});
		return res;
	}
  },
  mounted() {
	this.axios.get("https://boardgamegeek.com/xmlapi2/hot?type=boardgame")
		.then(rsp => {
			if (rsp.status === 200) {
				let data = this.parseXML(rsp.data);
				this.boardgames = data.items.item;
			} else {
				console.log('error fetch: '+ rsp.statusText);
			}
		})
		.catch(err => {
			console.log(err);
		})
		.then(() => {
			console.log('dou?');
		})
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
/*li {
  display: inline-block;
  margin: 0 10px;
}*/
a {
  color: #42b983;
}
input {
	padding: 15px;
	border: 2px solid #eaeaea;
	shadow-box: none;
}
</style>
