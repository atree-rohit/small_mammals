<style scoped>
	.table-area{
		width: 100%;
		height: 80vh;
		overflow: scroll;
	}
	.table-area .table td, .table-area .table th{
		white-space: nowrap;
	}
	.link-cell:hover{
		cursor: pointer;
		background: rgba(255,255,0,.7) !important;
	}

</style>
<template>
	<div>
		<div class="text-center py-3">
			<button 
				class="btn mx-3"
				v-for="(d,s) in allData"
				@click="selectedSource = s"
				:class="sourceBtnClass(s)"
			>
			{{portal_names[s]}} ({{d.length}})
			</button>
		</div>
		<div class="table-area bg-light">
			<table class="table table-sm table-striped">
				<thead>
					<tr class="bg-dark text-light">
						<th  v-for="(v,k) in allData[selectedSource][0]" v-text="headerName(k)" v-if="skipFields.indexOf(k) == -1"></th>
					</tr>
				</thead>
				<tbody class="">
					<!--  -->
					<tr v-for="row in allData[selectedSource]" >
						<template v-for="(cell, k) in row" v-if="skipFields.indexOf(k) == -1">
							<td v-if="k=='id'" v-text="cell" @click="openLink(row)" class="text-primary link-cell"></td>
							<td v-else v-text="cell"></td>
						</template>
					</tr>
				</tbody>
			</table>
		</div>
	</div>
</template>
<script>
	import inat_raw from './data/inat_small_mammals.json'
	import ibp_raw from './data/ibp_small_mammals.json'

	

	export default {
		name: 'app',
		data () {
			return {
				allData:{
					inat: inat_raw,
					ibp:ibp_raw
				},
				portal_names:{
					inat:"iNaturalist",
					ibp:"India Biodiversity Portal"
				},
				selectedSource:"inat",
				skipFields:["observed_on_string", "time_observed_at", "time_zone", "user_id", "quality_grade", "license", "url", "created_at","updated_at","image_url","sound_url","tag_list","description","num_identification_agreements","num_identification_disagreements","captive_cultivated","oauth_application_id", "positional_accuracy","private_place_guess","private_latitude","private_longitude","public_positional_accuracy","geoprivacy","taxon_geoprivacy","coordinates_obscured","positioning_method","positioning_device","species_guess", "iconic_taxon_name","taxon_id", "taxon_variety_name","taxon_form_name","flagNotes","noOfIdentifications","geoPrivacy", "taxon_superclass_name", "taxon_supertribe_name", "taxon_supertribe_name", "taxon_genushybrid_name", "taxon_hybrid_name", "associatedMedia","__1","reverseGeocodedName","state","tahsil","district","__2","toDate__1","observedInMonth	__3", "taxon_kingdom_name", "taxon_phylum_name", "taxon_subphylum_name", "taxon_superfamily_name", "taxon_subtribe_name", "taxon_subspecies_name", "createdOn","group_id", "locationScale", "toDate", "kingdom","phylum", "superfamily", "observedInMonth", "private_place", "subclass", "superorder"]
			}
		},
		mounted(){
		},
		methods:{
			sourceBtnClass(s){
				let op ="btn-outline-primary bg-light";
				if(s == this.selectedSource){
					op = "btn-success";
				}
				return op;
			},
			headerName(n){
				let op = n.replaceAll("_", " ");

				return this.titleCase(op);
			},
			titleCase(str) {
				return str.toLowerCase().split(' ').map(function(word) {
					return word.replace(word[0], word[0].toUpperCase());
				}).join(' ');
			},
			openLink(row){
				if(this.selectedSource == "inat"){
					window.open(`https://www.inaturalist.org/observations/${row.id}`, '_blank').focus();
				} else {
					window.open(`https://indiabiodiversity.org/observation/show/${row.id}`, '_blank').focus();
				}
			}
		}
	}	
</script>
