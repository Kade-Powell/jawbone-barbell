<template>
  <div v-if="dataLoaded" class="container mt-10 px-4">
    <!-- no data? -->
    <div v-if="data.length === 0" class="w-full flex flex-col items-center">
      <h1 class="text-2xl">Looks Empty....</h1>
      <router-link
        :to="{ name: 'Create' }"
        class="mt-6 py-2 px-6 rounded-sm text-sm text-white  bg-at-green
         duaration-200 border-solid border-2 border-transparent
         hover:border-at-green hover:bg-transparent hover:text-at-green"
        >Create</router-link
      >
    </div>

    <!-- data -->
    <div
      v-else
      class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6"
    >
      <router-link
        class="flex flex-col items-center p-8 shadow-lg bg-black cursor-pointer hover:border-white border-transparent border-2"
        :to="{ name: 'View-Workout', params: { workoutId: workout.id } }"
        v-for="(workout, index) in data"
        :key="index"
      >
        <img
          v-if="workout.workoutType === 'cardio'"
          src="@/assets/images/running-light.png"
          class="h-24 w-auto"
          alt=""
        />
        <img
          v-if="workout.workoutType === 'strength'"
          src="@/assets/images/dumbbell-light.png"
          class="h-24 w-auto"
          alt=""
        />

        <p
          class="mt-6 py-1 px-3 text-xs text-white bg-jb-red shadow-lg rounded-lg"
        >
          {{ workout.workoutType }}
        </p>
        <h1 class="mt-8 mb-2 text-center text-xl">
          {{ workout.workoutName }}
        </h1>
      </router-link>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'
import { supabase } from '../supabase/init'

export default {
  name: 'Home',
  components: {},
  setup() {
    // Create data / vars
    const data = ref([])
    const dataLoaded = ref(null)
    // Get data
    const getData = async () => {
      try {
        const { data: workouts, error } = await supabase
          .from('workouts')
          .select('*')

        if (error) throw error

        data.value = workouts
        dataLoaded.value = true
      } catch (error) {
        console.warn(error.message)
      }
    }
    // Run data function
    getData()
    return { data, dataLoaded, getData }
  }
}
</script>
