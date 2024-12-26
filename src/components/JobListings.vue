<script setup>
import JobListing from "@/components/JobListing.vue";
//import jobData from "@/jobs2.json";
import axios from "axios";
//import { ref, defineProps, onMounted } from "vue";
import { reactive, defineProps, onMounted } from "vue";
import PulseLoader from "vue-spinner/src/PulseLoader.vue";

defineProps({
  limit: Number,
  showButton: {
    type: Boolean,
    default: false,
  },
});

//using json file
//const jobs = ref(jobData);

//using ref
/*const jobs = ref([]);
onMounted(async () => {
  try {
    const response = await axios.get("http://localhost:5000/jobs");
    jobs.value = response.data;
  } catch (error) {
    console.error("Error fetching jobs");
  }
});*/
//console.log(jobs.value);

//using reactive
const state = reactive({
  jobs: [],
  isLoading: true,
});

onMounted(async () => {
  try {
    const response = await axios.get("/api/jobs");
    state.jobs = response.data;
  } catch (error) {
    console.error("Error fetching jobs", error);
  } finally {
    state.isLoading = false;
  }
});
</script>

<template>
  <section class="bg-blue-50 px-4 py-10">
    <div class="conatiner-xl" lg:container m-auto>
      <h2 class="text-3xl font-bold text-green-500 mb-6 text-center">
        Browse Jobs
      </h2>
      <!--Show Loading Spinner while loading is true-->
      <div v-if="state.isLoading" class="text-center text-gray-500 py-6">
        <PulseLoader />
      </div>

      <!--Show job Listing when done loading-->
      <div v-else class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <!-- <JobListing
          v-for="job in jobs.slice(0, limit || jobs.length)"
          :key="job.id"
          :job="job"
        /> -->
        <JobListing
          v-for="job in state.jobs.slice(0, limit || state.jobs.length)"
          :key="job.id"
          :job="job"
        />
      </div>
    </div>
  </section>
  <section v-if="showButton" class="m-auto max-w-lg my-10 px-6">
    <a
      href="/jobs"
      class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
      >View All Jobs</a
    >
  </section>
</template>
