<template>
  <div class="max-w-screen-md mx-auto px-4 py-10">
    <!-- status msg -->
    <div
      v-if="statusMsg || errorMsg"
      class="mb-10 p-4 border-2 rounded-md  shadow-lg"
    >
      <p class="text-at-green">{{ statusMsg }}</p>
      <p class="text-red-500">{{ errorMsg }}</p>
    </div>

    <!-- create form -->
    <div class="p-8 flex item-start bg-black rounded-md shadow-lg">
      <form
        @submit.prevent="createWorkout"
        class="flex flex-col gap-y-5 w-full"
      >
        <h1 class="text-2xl">Record Workout</h1>

        <!-- workout name -->
        <div class="flex flex-col">
          <label for="workout-name" class="mb-1 text-sm">Workout Name</label>
          <input
            class="p-2 text-black "
            type="text"
            id="workout-name"
            v-model="workoutName"
            required
          />
        </div>
        <!-- workout type -->
        <div class="flex flex-col">
          <label for="workout-type" class="mb-1 text-sm">Workout Type</label>
          <select
            @change="workoutChange"
            class="p-2  text-black"
            id="workout-type"
            required
            v-model="workoutType"
          >
            <option value="">Select Workout</option>
            <option value="strength">Strength Training</option>
            <option value="cardio">Cardio Training</option>
          </select>
        </div>

        <!-- IF strength training inputs -->
        <div v-if="workoutType === 'strength'" class="flex flex-col gap-y-4">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row"
            v-for="(item, index) in exercises"
            :key="index"
          >
            <div class="flex flex-col md:w-1/3">
              <label for="exercise-name" class="mb-1 text-sm">Exercise</label>
              <input
                type="text"
                required
                class="p-2 w-full text-gray-500 "
                v-model="item.exercise"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label for="sets" class="mb-1 text-sm">Sets</label>
              <input
                type="text"
                required
                class="p-2 w-full text-gray-500 "
                v-model="item.sets"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label for="reps" class="mb-1 text-sm">Reps</label>
              <input
                type="text"
                required
                class="p-2 w-full text-gray-500 "
                v-model="item.reps"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label for="weight" class="mb-1 text-sm">Weight</label>
              <input
                type="text"
                required
                class="p-2 w-full text-gray-500 "
                v-model="item.weight"
              />
            </div>

            <img
              @click="deleteExercise(item.id)"
              src="@/assets/images/trash-light.png"
              class="h-4 w-auto absolute -left-5 cursor-pointer"
              alt=""
            />
          </div>

          <button
            @click="addExercise"
            type="button"
            class="mt-6 py-2 px-6 rounded-sm self-start text-sm text-white  bg-at-green
         duaration-200 border-solid border-2 border-transparent
         hover:border-at-green hover:bg-transparent hover:text-at-green"
          >
            Add Exercise
          </button>
        </div>

        <!-- IF cardio training inputs -->
        <div v-if="workoutType === 'cardio'" class="flex flex-col gap-y-4">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row"
            v-for="(item, index) in exercises"
            :key="index"
          >
            <div class="flex flex-col md:w-1/3">
              <label for="cardio-type" class="mb-1 text-sm">Type</label>
              <select
                id="cardio-type"
                class="p-2 w-full text-gray-500 "
                v-model="item.cardioType"
              >
                <option value="">Select Type</option>
                <option value="run">Run</option>
                <option value="walk">Walk</option>
              </select>
            </div>

            <div class="flex flex-col flex-1">
              <label for="distance" class="mb-1 text-sm">Distance</label>
              <input
                type="text"
                required
                class="p-2 w-full text-gray-500 "
                v-model="item.distance"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label for="duration" class="mb-1 text-sm">Duration</label>
              <input
                type="text"
                required
                class="p-2 w-full text-gray-500 "
                v-model="item.duration"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label for="pace" class="mb-1 text-sm">Pace</label>
              <input
                type="text"
                required
                class="p-2 w-full text-gray-500 "
                v-model="item.pace"
              />
            </div>

            <img
              @click="deleteExercise(item.id)"
              src="@/assets/images/trash-light.png"
              class="h-4 w-auto absolute -left-5 cursor-pointer"
              alt=""
            />
          </div>

          <button
            @click="addExercise"
            type="button"
            class="mt-6 py-2  px-6 rounded-sm self-start text-sm text-white  bg-at-green
         duaration-200 border-solid border-2 border-transparent
         hover:border-at-green hover:bg-transparent hover:text-at-green"
          >
            Add Exercise
          </button>
        </div>

        <button
          type="submit"
          class="mt-6 py-2 w-full px-6 rounded-sm self-start text-sm text-white  bg-at-green
         duaration-200 border-solid border-2 border-transparent
         hover:border-at-green hover:bg-transparent hover:text-at-green"
        >
          Record Workout
        </button>
      </form>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'
import { uid } from 'uid'
import { supabase } from '../supabase/init'

export default {
  name: 'create',
  setup() {
    // Create data
    const workoutName = ref('')
    const workoutType = ref('')
    const exercises = ref([])
    const statusMsg = ref(null)
    const errorMsg = ref(null)
    // Add exercise
    const addExercise = () => {
      if (workoutType.value === 'strength') {
        exercises.value.push({
          id: uid(),
          exercise: '',
          sets: '',
          reps: '',
          weight: ''
        })
      } else if (workoutType.value === 'cardio') {
        exercises.value.push({
          id: uid(),
          cardioType: '',
          distance: '',
          duration: '',
          pace: ''
        })
      }
    }
    // Delete exercise
    const deleteExercise = (id) => {
      if (exercises.value.length > 1) {
        exercises.value = exercises.value.filter((exercise) => {
          exercise.id !== id
        })
      } else if (exercises.value.length <= 1) {
        errorMsg.value =
          'Error: Cannot remove, need to have at least one exercise'
        setTimeout(() => {
          errorMsg.value = null
        }, 5000)
      }
    }
    // Listens for chaging of workout type input
    const workoutChange = () => {
      exercises.value = []
      addExercise()
    }
    // Create workout
    const createWorkout = async () => {
      try {
        const { error } = await supabase.from('workouts').insert([
          {
            workoutName: workoutName.value,
            workoutType: workoutType.value,
            exercises: exercises.value
          }
        ])
        if (error) throw error

        statusMsg.value = 'Success: Workout Created!'
        setTimeout(() => {
          statusMsg.value = null
        }, 5000)

        workoutName.value = null
        workoutType.value = ''
        exercises.value = []
      } catch (error) {
        errorMsg.value = `Error: ${error.errorMsg}`
        setTimeout(() => {
          errorMsg.value = null
        }, 5000)
      }
    }
    return {
      workoutName,
      workoutType,
      exercises,
      statusMsg,
      errorMsg,
      addExercise,
      workoutChange,
      deleteExercise,
      createWorkout
    }
  }
}
</script>
