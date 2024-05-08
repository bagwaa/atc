<script setup>
import { ref } from 'vue'

const airport = ref('')
const destination = ref('')
const aircraft = ref('')
const callsign = ref('')
const gate = ref('')
const information = ref('')
const runway = ref('')
const qnh = ref('')
const holding_point = ref('')

const sid = ref('')
const runway_cleared = ref('')
const squak = ref('')
const face = ref('')
const taxi_via = ref('')

const showClearancePhrase =
  airport && destination && aircraft && callsign && gate && information && runway && qnh

const haveBeenCleared = sid && runway_cleared && squak

const showPushbackPhrase = ref(false)
const showRequestTaxiPhrase = ref(false)
const showTaxiInProgressPhrase = ref(false)
const readyForPushbackAndStartOnwards =
  airport &&
  callsign &&
  gate &&
  aircraft &&
  information &&
  runway &&
  qnh &&
  destination &&
  sid &&
  runway_cleared &&
  squak
</script>

<template>
  <div class="container p-8">
    <h1 class="text-3xl font-bold">VATSIM Helper</h1>
    <p class="my-2">
      A little helper to capture ATC data and generate phrases I want to say on VATSIM because I am
      shit scared and don't want to panic.
    </p>
    <div v-show="readyForPushbackAndStartOnwards">
      <button
        @click="showPushbackPhrase = !showPushbackPhrase"
        class="bg-blue-700 hover:bg-blue-700 text-white py-2 px-4 rounded mr-3 mb-1"
      >
        Pushback and Start
      </button>
      <button
        @click="showRequestTaxiPhrase = !showRequestTaxiPhrase"
        class="bg-blue-700 hover:bg-blue-700 text-white py-2 px-4 rounded mr-3 mb-1"
      >
        Ready for Taxi
      </button>
      <button
        @click="showTaxiInProgressPhrase = !showTaxiInProgressPhrase"
        class="bg-blue-700 hover:bg-blue-700 text-white py-2 px-4 rounded"
      >
        Call Tower
      </button>
    </div>

    <h2 class="my-2 text-lg font-bold">General</h2>

    <div class="w-full">
      <div class="flex my-2">
        <div class="w-1/2">Airport</div>
        <div class="w-1/2">
          <input v-model="airport" class="p-1 border border-gray-400 rounded" type="text" />
        </div>
      </div>
      <div class="flex my-2">
        <div class="w-1/2">Destination</div>
        <div class="w-1/2">
          <input v-model="destination" class="p-1 border border-gray-400 rounded" type="text" />
        </div>
      </div>
      <div class="flex my-2">
        <div class="w-1/2">Aircraft</div>
        <div class="w-1/2">
          <input v-model="aircraft" class="p-1 border border-gray-400 rounded" type="text" />
        </div>
      </div>
      <div class="flex my-2">
        <div class="w-1/2">Callsign</div>
        <div class="w-1/2">
          <input v-model="callsign" class="p-1 border border-gray-400 rounded" type="text" />
        </div>
      </div>
      <div class="flex my-2">
        <div class="w-1/2">Gate / Stand</div>
        <div class="w-1/2">
          <input v-model="gate" class="p-1 border border-gray-400 rounded" type="text" />
        </div>
      </div>
    </div>

    <h2 class="my-2 text-lg font-bold">ATIS</h2>

    <div class="w-full">
      <div class="flex my-2">
        <div class="w-1/2">Information</div>
        <div class="w-1/2">
          <input v-model="information" class="p-1 border border-gray-400 rounded" type="text" />
        </div>
      </div>
      <div class="flex my-2">
        <div class="w-1/2">Runway</div>
        <div class="w-1/2">
          <input v-model="runway" class="p-1 border border-gray-400 rounded" type="text" />
        </div>
      </div>
      <div class="flex my-2">
        <div class="w-1/2">QNH</div>
        <div class="w-1/2">
          <input v-model="qnh" class="p-1 border border-gray-400 rounded" type="text" />
        </div>
      </div>
    </div>

    <div v-show="showClearancePhrase">
      <h2 class="my-2 text-lg font-bold">Request Clearence</h2>

      <p class="text-green-800 text-xl italic p-8 border border-green-500 rounded-3xl my-2">
        Request: "{{ airport }} ground, this is {{ callsign }} at stand {{ gate }} in a
        {{ aircraft }} with information {{ information }}, QNH {{ qnh }} requesting IFR clearance to
        {{ destination }}"
      </p>

      <div class="w-full">
        <div class="flex my-2">
          <div class="w-1/2">SID</div>
          <div class="w-1/2">
            <input v-model="sid" class="p-1 border border-gray-400 rounded" type="text" />
          </div>
        </div>
        <div class="flex my-2">
          <div class="w-1/2">Runway Cleared</div>
          <div class="w-1/2">
            <input
              v-model="runway_cleared"
              class="p-1 border border-gray-400 rounded"
              type="text"
            />
          </div>
        </div>
        <div class="flex my-2">
          <div class="w-1/2">Squak</div>
          <div class="w-1/2">
            <input v-model="squak" class="p-1 border border-gray-400 rounded" type="text" />
          </div>
        </div>
      </div>
      <p
        v-if="haveBeenCleared"
        class="text-green-800 text-xl italic p-8 border border-green-500 rounded-3xl my-2"
      >
        Readback: "Depart runway {{ runway_cleared }}, via SID {{ sid }}, squak {{ squak }}
        {{ callsign }}"
      </p>
    </div>
    <div v-show="showPushbackPhrase">
      <p class="text-green-800 text-xl italic p-8 border border-green-500 rounded-3xl my-2">
        Request: "{{ airport }} ground, {{ callsign }} ready for push and start.
      </p>
      <div class="w-full">
        <div class="flex my-2">
          <div class="w-1/2">Face Direction</div>
          <div class="w-1/2">
            <input v-model="face" class="p-1 border border-gray-400 rounded" type="text" />
          </div>
        </div>
      </div>
      <p
        v-if="face"
        class="text-green-800 text-xl italic p-8 border border-green-500 rounded-3xl my-2"
      >
        Readback: "Pushback approved, face {{ face }}, {{ callsign }}"
      </p>
    </div>
    <div v-show="showRequestTaxiPhrase">
      <p class="text-green-800 text-xl italic p-8 border border-green-500 rounded-3xl my-2">
        Request: "{{ airport }} ground, {{ callsign }} ready for taxi.
      </p>
      <div class="w-full">
        <div class="flex flex-col my-2">
          <div class="w-full my-2">Taxi Via</div>
          <div class="w-full">
            <textarea
              v-model="taxi_via"
              class="p-1 border w-full border-gray-400 rounded"
              type="text"
            />
          </div>
        </div>
      </div>
      <div>
        <p
          v-if="taxi_via"
          class="text-green-800 text-xl italic p-8 border border-green-500 rounded-3xl my-2"
        >
          Readback: "Taxi to runway {{ runway_cleared }}, via {{ taxi_via }}, {{ callsign }}"
        </p>
      </div>
    </div>
    <div v-show="showTaxiInProgressPhrase">
      <p class="text-green-800 text-xl italic p-8 border border-green-500 rounded-3xl my-2">
        Request: "{{ airport }} tower, hello, {{ callsign }} taxing to runway {{ runway_cleared }}.
      </p>
    </div>
  </div>
</template>
