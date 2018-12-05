<template>
  <div class="text-center py-5">
    <h1 class="display-4 mb-3">Board Game Finder</h1>
	<form class="mb-3">
        <div class="input-group justify-content-center">
		    <input id="keyword" type="text" v-model="keyword">
            <div class="input-group-append rounded-0">
                <button class="input-group-btn btn btn-outline-secondary rounded-0">&times;</button>
            </div>
        </div>
	</form>
	<p v-if="keyword.length === 0" class="text-muted f-small">Start typing to search your favorite Board Games (WIP)</p>

    <h2 class="font-weight-light mt-5 mb-4">Hot List</h2>
	<div class="row">
		<div v-for="b in boardgames" class="col-2">
            <a target="_blank" :href="getURL(b.$.id)">
                <div class="card">
                    <img class="card-img-top" :src="b.thumbnail[0].$.value">
                    <div class="card-body">
                        {{b.name[0].$.value}}
                    </div>
                </div>
            </a>
		</div>
	</div>
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
	},
    getURL: function(id) {
        return 'https://boardgamegeek.com/boardgame/'+ id +'';
    }
  },
  mounted() {
	this.axios.get("https://boardgamegeek.com/xmlapi2/hot?type=boardgame")
		.then(rsp => {
			if (rsp.status === 200) {
				let data = this.parseXML(rsp.data);
				this.boardgames = data.items.item;
                console.log(this.boardgames);
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
</style>
