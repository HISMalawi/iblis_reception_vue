<template>
  <div class="search-card custom-bg card tile is-child">
    <div class="card-content control">
      <div>
        <div class="field has-addons">
          <div class="control">
            <input
              class="input is-small"
              type="text"
              placeholder="Search Patient"
              v-model="searchString"
            />
          </div>
          <div class="control">
            <a @click="Search" class="button is-primary is-small"> Search </a>
          </div>
        </div>
      </div> 
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent, ref, watch } from "vue";
import toggleViews from "@/composables/toggleViews";
import SearchPatient from "@/composables/searchPatient";
import { Patient } from "@/interfaces/Patient";
import { MutationTypes, useStore } from "@/store";

export default defineComponent({
  name: "SearchPanel",
  setup() {
    const store = useStore();

    const defaultPatient: Patient = {
      address: "",
      created_at: "",
      created_by: 0,
      deleted_at: "",
      dob: "",
      dob_estimated: 0,
      email: "",
      external_patient_number: "",
      first_name_code: "",
      gender: 0,
      id: 0,
      last_name_code: "",
      name: "",
      patient_number: "",
      phone_number: "",
      updated_at: "",
    };

    const searchString = ref<string>("");

    const { search, patients } = SearchPatient();

    const {
      OpenPatientSearchResultsView,
      ClosePatientSearchResultsView,
      OpenPatientDetails,
    } = toggleViews();

    const Search = () => {
      
      patients.value.length = 0

      if (searchString.value == "") {
      } else {

        store.commit(MutationTypes.SEARCH_PATIENT_IN_PROGRESS, true);

        search(searchString.value);
        OpenPatientDetails(false);
        OpenPatientSearchResultsView();
      }

      store.commit(MutationTypes.SET_SELECTED_PATIENT, defaultPatient);
    };

    watch(
      () => [searchString.value],
      () => {
        setTimeout(() => {
          if (searchString.value == "") {
            OpenPatientDetails(false);
            ClosePatientSearchResultsView();
          } else {
            // search(searchString.value);
            OpenPatientDetails(false);
            //OpenPatientSearchResultsView();
          }

          store.commit(MutationTypes.SET_SELECTED_PATIENT, defaultPatient);
        }, 500);
      }
    );

    return { Search, searchString };
  },
});
</script>
<style>
.search-card {
  max-height: 80px !important;
  max-width: 530px !important;
  margin: 0 auto;
  
}
</style>
