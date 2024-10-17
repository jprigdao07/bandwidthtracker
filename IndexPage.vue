<template>
  <q-page>
    <!-- Display the bandwidth information -->
    <div v-if="state.bandwidthData" class="q-mt-md">
      <q-card>
        <q-card-section>
          <div>
            <strong>Interface: <span style="color: lime;">{{ state.bandwidthData.alias }}</span></strong> 
          </div>
          <div>
            <strong>Speed: <span style="color: lime;">{{ state.bandwidthData.speed }}</span></strong>
          </div>
          <div>
            <strong>Download Bandwidth: <span style="color: lime;">{{ formatBandwidth(state.bandwidthData.downband) }}</span></strong>
          </div>
          <div>
            <strong>Upload Bandwidth: <span style="color: lime;">{{ formatBandwidth(state.bandwidthData.upband) }}</span></strong>
          </div>
          <div>
            <strong>MTU: <span style="color: lime;">{{ state.bandwidthData.mtu }}</span></strong>
          </div>
          <div>
            <strong>IP Address: <span style="color: lime;">{{ state.bandwidthData.ipAddr }}</span></strong>
          </div>
        </q-card-section>
      </q-card>
    </div>

    <!-- Radar Chart for Wi-Fi Experience -->
    <div class="q-pa-md">
      <q-card class="my-card">
        <q-card-section>
          <h4 class="text-h4">Wi-Fi Experience</h4>
          <canvas id="wifiExperienceChart"></canvas>
        </q-card-section>
      </q-card>
      <div class="q-pa-md center-container">
      <!-- Round Button to trigger random bandwidth data -->
      <q-btn
        icon="power_settings_new"
        color="info"
        round
        size="lg"
        class="round-button animated-button"
        @click="generateRandomBandwidthData"
      />
    </div>
    </div>
  </q-page>
</template>

<script>
import { reactive, onMounted, onBeforeUnmount } from 'vue';
import { Chart, RadarController, RadialLinearScale, PointElement, LineElement, Filler, Tooltip, Legend } from 'chart.js';

export default {
  setup() {
    const state = reactive({
      bandwidthData: null,
    });

    // Generate random bandwidth data
    const generateRandomBandwidthData = () => {
      const randomData = {
        alias: 'Gi0/0',
        speed: '100M',
        downband: Math.floor(Math.random() * 2000000), 
        upband: Math.floor(Math.random() * 2000000),   
        mtu: 1500,
        ipAddr: '42.200.231.215',
      };

      state.bandwidthData = randomData;

     
      updateRadarChart();
    };

    const formatBandwidth = (bandwidth) => {
      return bandwidth >= 1000000
        ? (bandwidth / 1000000) + " Gbps"
        : (bandwidth / 1000) + " Mbps";
    };

    let radarChart;

    const updateRadarChart = () => {
      const ctx2 = document.getElementById('wifiExperienceChart').getContext('2d');
      const data = [
        Math.floor(Math.random() * 1000),
        Math.floor(Math.random() * 1000),
        Math.floor(Math.random() * 1000),
        Math.floor(Math.random() * 1000),
        Math.floor(Math.random() * 1000)
      ];

      if (radarChart) {
        radarChart.data.datasets[0].data = data;
        radarChart.update();
      } else {
        radarChart = new Chart(ctx2, {
          type: 'radar',
          data: {
            labels: [
              'Successful Connects',
              'Throughput',
              'Coverage',
              'Capacity and Interference',
              'Roaming'
            ],
            datasets: [{
              label: 'Wi-Fi Experience',
              data: data,
              backgroundColor: 'rgba(0, 0, 128, 0.2)',
              borderColor: 'rgba(0, 0, 128, 1)', 
              pointBackgroundColor: 'rgba(0, 0, 128, 1)', 
            }]
          },
          options: {
            scales: {
              r: {
                suggestedMin: 0,
                suggestedMax: 100,

                pointLabels: {
                  font: {
                    size: 16, 
                  },
                }
              }
            }
          }
        });
      }
    };

    let intervalId = null;

    onMounted(() => {
      Chart.register(RadarController, RadialLinearScale, PointElement, LineElement, Filler, Tooltip, Legend);
      generateRandomBandwidthData();

      intervalId = setInterval(generateRandomBandwidthData, 1000);
    });

    onBeforeUnmount(() => {
     
      clearInterval(intervalId);
    });

    return {
      state,
      generateRandomBandwidthData,
      formatBandwidth,
    };
  }
};
</script>

