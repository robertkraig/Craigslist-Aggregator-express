<template>
  <div v-if="isConfLoaded">
    <form v-on:submit.prevent="submit" id="find_items">
      <div id="change_size_container">
        <div v-html="getPageTitle" style="font-size: 20px;"></div>
        <template v-for="(field, fIdx) in getFields">
          <div :key="fIdx">
            <div v-if="isEq(field.argType, 'text')">
              <label class="fields" :for="field.argId" v-html="field.argTitle"></label>
              <input class="fields" type="text" v-model="form[field.argName]" :id="field.argId"/>
              <br style="margin:0;padding:0; height:1px; clear: left;"/>
            </div>
            <div v-if="isEq(field.argType, 'radio')">
              <template v-for="(radio, rIdx) in field.radios">
                <div :key="rIdx">
                  <label class="fields" :for="radio.argNameId" v-html="radio.argName"></label>
                  <input class="fields" type="radio" v-model="form[field.argName]" :value="radio.arg"
                         :id="radio.argNameId"/>
                  <br style="margin:0;padding:0; height:1px; clear: left;"/>
                </div>
              </template>
            </div>
            <div v-if="isEq(field.argType, 'checkbox')">
              <label class="fields" :for="field.checkbox.argName" v-html="field.checkbox.title"></label>
              <input class="fields" type="checkbox" v-model="form[field.checkbox.argName]" :value="field.checkbox.value"
                     :id="field.checkbox.argName"/>
              <br style="margin:0;padding:0; height:1px; clear: left;"/>
            </div>
          </div>
        </template>
        <cite v-html="getSearchExample"></cite>
        <div id="locations_container">
          Region:&nbsp;&nbsp;
          <a v-on:click.prevent="toggleRegionList">
            <span v-if="isRegionListOpen">close</span>
            <span v-if="!isRegionListOpen">open</span>
          </a>
          <div :class="{open:isRegionListOpen}" id="region_list">
            <template v-for="region in getRegionList">
              <label :for="region.type" :key="region.type">
                <input v-on:click="updateRegionSelection(region)" v-model="region.selected"
                       class="regions" type="checkbox" :id="region.type" :value="region.type"/>{{region.name}}
              </label>
            </template>
          </div>
          <br/>
          Areas:&nbsp;&nbsp;
          <a v-on:click.prevent="toggleAreaList">
            <span v-if="isAreaListOpen">close</span>
            <span v-if="!isAreaListOpen">open</span>
          </a>
          <div :class="{open:isAreaListOpen}" id="areas_list">
            <label>Selected selected: {{getTotalAreas}}</label>
            <template v-for="area in getSelectedAreas">
              <label :for="area.partial" :key="area.partial">
                <input v-on:click="updateAreaSelection(area)" class="region location"
                       :class="{[area.type]:true}" v-model="area.selected" type="checkbox" :id="area.partial"
                       :value="area.partial"/>{{area.name}}, {{area.state}}
              </label>
            </template>
            <label>Un-Selected</label>
            <template v-for="area in getUnselectedAreas">
              <label :for="area.partial" :key="area.partial">
                <input v-on:click="updateAreaSelection(area)" class="region location"
                       :class="{[area.type]:true}" v-model="area.selected" type="checkbox" :id="area.partial"
                       :value="area.partial"/>{{area.name}}, {{area.state}}
              </label>
            </template>
          </div>
        </div>
        <button id="search_btn" type="submit">Search</button>
      </div>
    </form>
  </div>
</template>

<script>
import {mapActions, mapGetters, mapMutations} from 'vuex'

export default {
  props: {
    initFormFields: {
      required: true
    }
  },
  data () {
    return {
      form: this.initFormFields
    }
  },
  methods: {
    ...mapActions([
      'submitSearch'
    ]),
    ...mapMutations([
      'toggleRegionList',
      'toggleAreaList',
      'updateRegionSelection',
      'updateAreaSelection'
    ]),
    isEq (field, val) {
      return field === val
    },
    submit () {
      this.submitSearch({
        form: this.form
      })
    }
  },
  computed: {
    ...mapGetters([
      'isAreaListOpen',
      'isConfLoaded',
      'isRegionListOpen',
      'getSearchData',
      'getPageTitle',
      'getSearchExample',
      'getRegionList',
      'getAreaList',
      'getFields',
      'getSelectedAreas',
      'getUnselectedAreas',
      'getTotalAreas'
    ])
  }
}
</script>
