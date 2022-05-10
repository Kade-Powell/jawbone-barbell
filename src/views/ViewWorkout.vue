<template>
  <div class="max-w-screen-sm mx-auto px-4 py-10">
    <!-- App MSG -->
    <div
      v-if="statusMsg || errorMsg"
      class="mb-10 p-4 rounded-md shadow-lg bg-light-grey"
    >
      <p class="text-at-green">{{ statusMsg }}</p>
      <p class="text-red-500">{{ errorMsg }}</p>
    </div>

    <div v-if="dataLoaded">
      <!-- general workout info -->
      <div
        class="flex flex-col items-center p-8 rounded-md shadow-lg bg-black relative"
      >
        <div v-if="user" class="flex absolute left-2 top-2 gap-x-2">
          <div
            @click="toggleEdit"
            class="h-7 w-7 rounded-full flex justify-center items-center cursor-pointer bg-jb-red shadow-lg"
          >
            <img
              src="@/assets/images/pencil-light.png"
              alt="edit"
              class="h-4 w-auto"
            />
          </div>

          <div
            @click="toggleEdit"
            class="h-7 w-7 rounded-full flex justify-center items-center cursor-pointer bg-jb-red shadow-lg"
          >
            <img
              src="@/assets/images/trash-light.png"
              alt="edit"
              class="h-4 w-auto"
            />
          </div>
        </div>
        <img
          v-if="data.workoutType === 'cardio'"
          src="@/assets/images/running-light.png"
          alt="icon"
          class="h-24 w-auto"
        />
        <img
          v-if="data.workoutType === 'strength'"
          src="@/assets/images/dumbbell-light.png"
          alt="icon"
          class="h-24 w-auto"
        />

        <span class="mt-6 py-1.5 px-5 text-xs bg-jb-red rounded-lg shadow-lg">{{
          data.workoutType
        }}</span>

        <!-- workout Name -->
        <div class="w-full mt-6">
          <input
            v-if="edit"
            class="p-2 w-full text-black"
            type="text"
            v-model="data.workoutName"
          />
          <h1 v-else class="text-jb-red text-2xl text-center">
            {{ data.workoutName }}
          </h1>
        </div>
      </div>

      <!-- Exercises -->
      <div
        class="mt-10 p-8 rounded-md flex flex-col items-center shadow-lg bg-black"
      >
        <!-- Strength training -->
        <div
          v-if="data.workoutType === 'strength'"
          class="flex flex-col gap-y-4 w-full"
        >
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
            v-for="(item, index) in data.exercises"
            :key="index"
          >
            <div class="flex flex-2 flex-col md:w-1/3">
              <label for="exercise-name" class="mb-1 text-sm text-jb-red"
                >Exercise</label
              >
              <input
                v-if="edit"
                id="exercise-name"
                class="p-2 w-full text-black"
                type="text"
                v-model="item.exercise"
              />
              <p v-else>{{ item.exercise }}</p>
            </div>

            <div class="flex flex-1 flex-col">
              <label for="sets" class="mb-1 text-sm text-jb-red">Sets</label>
              <input
                v-if="edit"
                id="sets"
                class="p-2 w-full text-black"
                type="text"
                v-model="item.sets"
              />
              <p v-else>{{ item.sets }}</p>
            </div>

            <div class="flex flex-1 flex-col">
              <label for="reps" class="mb-1 text-sm text-jb-red">Reps</label>
              <input
                v-if="edit"
                id="reps"
                class="p-2 w-full text-black"
                type="text"
                v-model="item.reps"
              />
              <p v-else>{{ item.reps }}</p>
            </div>

            <div class="flex flex-1 flex-col">
              <label for="weight" class="mb-1 text-sm text-jb-red"
                >Weight</label
              >
              <input
                v-if="edit"
                id="weight"
                class="p-2 w-full text-black"
                type="text"
                v-model="item.weight"
              />
              <p v-else>{{ item.weight }}</p>
            </div>

            <img
              v-if="edit"
              class="absolute h-4 w-auto -left-5 cursor-pointer"
              src="@/assets/images/trash-light.png"
              alt=""
            />
          </div>
        </div>

        <!-- Cardio training -->
        <div
          v-if="data.workoutType === 'cardio'"
          class="flex flex-col gap-y-4 w-full"
        >
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
            v-for="(item, index) in data.exercises"
            :key="index"
          >
            <div class="flex flex-2 flex-col md:w-1/3">
              <label for="cardioType" class="mb-1 text-sm text-jb-red"
                >Type</label
              >
              <select
                v-if="edit"
                id="cardioType"
                class="p-2 w-full text-black"
                type="text"
                v-model="item.cardioType"
              >
                <option value="">Select Type</option>
                <option value="run">Run</option>
                <option value="walk">Walk</option></select
              >
              <p v-else>{{ item.cardioType }}</p>
            </div>

            <div class="flex flex-1 flex-col">
              <label for="distance" class="mb-1 text-sm text-jb-red"
                >Distance</label
              >
              <input
                v-if="edit"
                id="distance"
                class="p-2 w-full text-black"
                type="text"
                v-model="item.distance"
              />
              <p v-else>{{ item.distance }}</p>
            </div>

            <div class="flex flex-1 flex-col">
              <label for="duration" class="mb-1 text-sm text-jb-red"
                >Duration</label
              >
              <input
                v-if="edit"
                id="duration"
                class="p-2 w-full text-black"
                type="text"
                v-model="item.duration"
              />
              <p v-else>{{ item.duration }}</p>
            </div>

            <div class="flex flex-1 flex-col">
              <label for="pace" class="mb-1 text-sm text-jb-red">Pace</label>
              <input
                v-if="edit"
                id="pace"
                class="p-2 w-full text-black"
                type="text"
                v-model="item.pace"
              />
              <p v-else>{{ item.pace }}</p>
            </div>

            <img
              v-if="edit"
              class="absolute h-4 w-auto -left-5 cursor-pointer"
              src="@/assets/images/trash-light.png"
              alt=""
            />
          </div>
        </div>

        <button
          v-if="user && edit"
          type="button"
          class="mt-6 py-2 px-6 rounded-sm self-start text-sm text-white  bg-at-green
         duaration-200 border-solid border-2 border-transparent
         hover:border-at-green hover:bg-transparent hover:text-at-green"
        >
          Add Exercise
        </button>
      </div>

      <button
        v-if="user && edit"
        type="button"
        class="mt-6 py-2 w-full px-6 rounded-sm self-start text-sm text-white  bg-at-green
         duaration-200 border-solid border-2 border-transparent
         hover:border-at-green hover:bg-transparent hover:text-at-green"
      >
        Update Workout
      </button>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue'
import { supabase } from '../supabase/init'
import { useRoute } from 'vue-router'
import store from '../store/index'
export default {
  name: 'view-workout',
  setup() {
    // Create data / vars
    const data = ref(null)
    const dataLoaded = ref(null)
    const errorMsg = ref(null)
    const statusMsg = ref(null)
    const route = useRoute()
    const user = computed(() => store.state.user)
    // Get current Id of route
    const currentId = route.params.workoutId
    // Get workout data
    const getData = async () => {
      try {
        const { data: workouts, error } = await supabase
          .from('workouts')
          .select('*')
          .eq('id', currentId)

        if (error) throw error

        data.value = workouts[0]
        dataLoaded.value = true
      } catch (error) {
        errorMsg.value = error.message
        setTimeout(() => {
          errorMsg.value = null
        }, 5000)
      }
    }
    getData()

    // Delete workout

    // Edit mode
    const edit = ref(null)

    const toggleEdit = () => {
      edit.value = !edit.value
    }
    // Add exercise

    // Delete exercise

    // Update Workout

    return { statusMsg, data, dataLoaded, errorMsg, edit, toggleEdit, user }
  }
}
</script>
