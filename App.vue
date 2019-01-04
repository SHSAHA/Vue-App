<template>
  <div id="app">
    <!-- <section>{{photos}}</section> -->
    <section>{{albums}}</section>
    <section class="section">
      <div class="container">
        <div class="content">
          <div class="box">
            <vue-scrolling-table
              :scroll-horizontal="scrollHorizontal"
              :scroll-vertical="scrollVertical"
              :sync-header-scroll="syncHeaderScroll"
              :sync-footer-scroll="syncFooterScroll"
              :dead-area-color="deadAreaColor"
              :class="{ freezeFirstColumn:freezeFirstColumn }"
            >
              <template slot="thead">
                <tr>
                  <th v-for="col in columns" :class="col.cssClasses" :key="col.id">{{ col.title }}</th>
                </tr>
              </template>
              <template slot="tbody">
                <tr v-for="item in photos" :key="item.id">
                  <td
                    v-for="col in columns"
                    :class="col.cssClasses"
                    :key="col.id"
                  >{{ item[col.id] }}</td>
                </tr>
              </template>
            </vue-scrolling-table>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>
<script>
import VueScrollingTable from "vue-scrolling-table"
import axios from "axios"
export default {
	name: "SampleApp",
	components: {
		VueScrollingTable,
	},
	mounted() {
		axios
			.get("http://jsonplaceholder.typicode.com/photos")
			.then(response => (this.photos = response))
		axios
			.get("http://jsonplaceholder.typicode.com/albums")
			.then(response => (this.albums = response))
	},
	data: function() {
		return {
			scrollVertical: true,
			scrollHorizontal: true,
			syncHeaderScroll: true,
			syncFooterScroll: true,
			deadAreaColor: "#FFFFFF",
			maxRows: 25,
			freezeFirstColumn: false,
			photos: null,
			albums: null,
			columns: [
				{ id: "id", title: "ALBUM ID", cssClasses: "" },
				{ id: "title", title: "ALBUM TITLE", cssClasses: "" },
				{ id: "photoTitle", title: "PHOTO TITLE", cssClasses: "" },
				{ id: "photoThumb", title: "PHOTO THUMBNAIL", cssClasses: "w2" },
			],
		}
	},
}
</script>
<style>
table.scrolling .w2 {
	width: 20em;
	min-width: 20em;
	max-width: 20em;
}

table.scrolling tfoot tr th {
	width: 130em;
	min-width: 130em;
	max-width: 130em;
}

table.freezeFirstColumn thead tr,
table.freezeFirstColumn tbody tr {
	display: block;
	width: min-content;
}

table.freezeFirstColumn thead td:first-child,
table.freezeFirstColumn tbody td:first-child,
table.freezeFirstColumn thead th:first-child,
table.freezeFirstColumn tbody th:first-child {
	position: sticky;
	position: -webkit-sticky;
	left: 0;
}

* {
	font-family: sans-serif;
}

.box {
	clear: both;
	padding: 0;
	min-height: 300px;
	height: 40vh;
	margin-left: auto;
	margin-right: auto;
	overflow: hidden;
}

@font-face {
	font-family: FontAwesome;
	src: url(https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/fonts/fontawesome-webfont.woff);
}

.fa {
	font-family: FontAwesome;
}
</style>
