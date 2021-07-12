<template>
	<div>
		<div>
			<button 
				class="btn" 
				v-for="(d,s) in allData"
				@click="selectedSource = s"
				:class="sourceBtnClass(s)"
			>
			{{s}} ({{d.length}})
			</button>
		</div>
		<table class="table table-sm table-striped table-hover">
			<thead>
				<tr>
					<td  v-for="(v,k) in allData[selectedSource][0]" v-text="k" v-if="skipFields.indexOf(k) == -1"></td>
				</tr>
			</thead>
			<tbody>
				<!-- @click="openLink(row)" -->
				<tr v-for="row in allData[selectedSource]" >
					<td v-for="(cell, k) in row" v-text="cell" v-if="skipFields.indexOf(k) == -1"></td>
				</tr>
			</tbody>
		</table>
	</div>
</template>
<script>
	import inat_raw from './data/inat.json'
	import ibp_sk_raw from './data/ibp_sk.json'
	import ibp_dj_raw from './data/ibp_dj.json'
	

	export default {
		name: 'app',
		data () {
			return {
				allData:{
					inat: inat_raw,
					ibp:ibp_dj_raw.concat(ibp_sk_raw)
				},
				selectedSource:"inat",
				skipFields:["observed_on_string", "time_observed_at", "time_zone", "user_id", "quality_grade", "license", "url", "created_at","updated_at","image_url","sound_url","tag_list","description","num_identification_agreements","num_identification_disagreements","captive_cultivated","oauth_application_id", "positional_accuracy","private_place_guess","private_latitude","private_longitude","public_positional_accuracy","geoprivacy","taxon_geoprivacy","coordinates_obscured","positioning_method","positioning_device","species_guess", "iconic_taxon_name","taxon_id", "taxon_variety_name","taxon_form_name","flagNotes","noOfIdentifications","geoPrivacy", "taxon_superclass_name", "taxon_supertribe_name", "taxon_supertribe_name", "taxon_genushybrid_name", "taxon_hybrid_name", "associatedMedia","__1","reverseGeocodedName","state","tahsil","district","__2","toDate__1","observedInMonth	__3"]
			}
		},
		mounted(){
		},
		methods:{
			sourceBtnClass(s){
				let op ="btn-outline-primary";
				if(s == this.selectedSource){
					op = "btn-success";
				}
				return op;

			},
			openLink(row){
				if(this.selectedSource == "inat"){
					window.open(`https://www.inaturalist.org/observations/${row.id}`, '_blank').focus();
				} else {
					window.open(`https://indiabiodiversity.org/observation/show/${row.catalogNumber}`, '_blank').focus();					
				}
			}
		}
	}	
</script>