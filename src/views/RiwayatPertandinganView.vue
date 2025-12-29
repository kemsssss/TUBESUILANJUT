<script setup lang="ts">
import AppNavbar from "../components/AppNavbar.vue";
import { ref, computed } from "vue";

const matches = ref([
  {
    id: 1,
    team: 'FC Warriors',
    sport: 'Futsal',
    date: '8 Januari 2025',
    time: '19:00',
    location: 'Lapangan Arena Sport, Jakarta Selatan',
    scheduledAt: '21 Desember 2024',
    status: 'upcoming', // upcoming | completed | cancelled
    outcome: null, // 'win' | 'loss' | 'draw' | null
    score: null
  },
  {
    id: 2,
    team: 'Thunder Squad',
    sport: 'Basket',
    date: '27 Desember 2024',
    time: '15:00',
    location: 'GOR Basket Senayan, Jakarta Pusat',
    scheduledAt: '14 Desember 2024',
    status: 'completed',
    outcome: 'win',
    score: '78-65'
  },
  {
    id: 3,
    team: 'Spike Masters',
    sport: 'Voli',
    date: '20 Desember 2024',
    time: '09:00',
    location: 'Lapangan Voli BSD, Tangerang Selatan',
    scheduledAt: '8 Desember 2024',
    status: 'cancelled',
    outcome: null,
    score: null
  },
  {
    id: 4,
    team: 'Dragon FC',
    sport: 'Futsal',
    date: '12 Januari 2025',
    time: '19:00',
    location: 'Futsal Corner, Bekasi',
    scheduledAt: '24 Desember 2024',
    status: 'upcoming',
    outcome: null,
    score: null
  },
  {
    id: 5,
    team: 'Eagles United',
    sport: 'Futsal',
    date: '23 Desember 2024',
    time: '17:00',
    location: 'Lapangan Senopati, Jakarta Selatan',
    scheduledAt: '16 Desember 2024',
    status: 'completed',
    outcome: 'draw',
    score: '3-3'
  },
  {
    id: 6,
    team: 'Net Ninjas',
    sport: 'Voli',
    date: '19 Desember 2024',
    time: '16:00',
    location: 'GOR Voli Cibubur, Jakarta Timur',
    scheduledAt: '10 Desember 2024',
    status: 'completed',
    outcome: 'loss',
    score: '1-3'
  }
]);

const query = ref('');
const filterStatus = ref('all');
const filterOutcome = ref('all');

const filtered = computed(() => {
  return matches.value.filter(m => {
    const q = query.value.toLowerCase();
    const matchesQuery = [m.team, m.location, m.sport].join(' ').toLowerCase().includes(q);
    const matchesStatus = filterStatus.value === 'all' ? true : m.status === filterStatus.value;
    const matchesOutcome = filterOutcome.value === 'all' ? true : (m.outcome === filterOutcome.value);
    return matchesQuery && matchesStatus && matchesOutcome;
  });
});

const counts = computed(() => ({
  total: matches.value.length,
  upcoming: matches.value.filter(m => m.status === 'upcoming').length,
  completed: matches.value.filter(m => m.status === 'completed').length,
  cancelled: matches.value.filter(m => m.status === 'cancelled').length,
  wins: matches.value.filter(m => m.outcome === 'win').length,
  losses: matches.value.filter(m => m.outcome === 'loss').length,
  draws: matches.value.filter(m => m.outcome === 'draw').length
}));

function cancelMatch(id: number) {
  const idx = matches.value.findIndex(m => m.id === id);
  if (idx !== -1) matches.value[idx].status = 'cancelled';
}
</script>

<template>
  <div class="page">
    <AppNavbar />

    <main class="history-page">
      <div class="hero">
        <h1>Riwayat Pertandingan</h1>
        <p class="subtitle">Kelola semua jadwal pertandingan dengan lawan</p>
      </div>

      <div class="history-content">
        <div class="stats-row">
          <div class="stat-card">
            <div class="stat-label">Total</div>
            <div class="stat-value">{{ counts.total }}</div>
          </div>

          <div class="stat-card blue">
            <div class="stat-label">Akan Datang</div>
            <div class="stat-value">{{ counts.upcoming }}</div>
          </div>

          <div class="stat-card green">
            <div class="stat-label">Selesai</div>
            <div class="stat-value">{{ counts.completed }}</div>
          </div>

          <div class="stat-card red">
            <div class="stat-label">Dibatalkan</div>
            <div class="stat-value">{{ counts.cancelled }}</div>
          </div>

          <div class="stat-card taller green">
            <div class="stat-label">Menang</div>
            <div class="stat-value">{{ counts.wins }}</div>
          </div>

          <div class="stat-card taller red">
            <div class="stat-label">Kalah</div>
            <div class="stat-value">{{ counts.losses }}</div>
          </div>

          <div class="stat-card taller grey">
            <div class="stat-label">Seri</div>
            <div class="stat-value">{{ counts.draws }}</div>
          </div>
        </div>

        <div class="search-bar">
          <input type="text" placeholder="Cari tim lawan, olahraga, atau lokasi..." v-model="query" />

          <div class="filters">
            <select v-model="filterStatus">
              <option value="all">Semua</option>
              <option value="upcoming">Akan Datang</option>
              <option value="completed">Selesai</option>
              <option value="cancelled">Dibatalkan</option>
            </select>
            <select v-model="filterOutcome">
              <option value="all">Semua</option>
              <option value="win">Menang</option>
              <option value="loss">Kalah</option>
              <option value="draw">Seri</option>
            </select>
          </div>
        </div>

        <div class="list">
          <div v-for="m in filtered" :key="m.id" class="match-card">
            <div class="match-left">
              <div class="team-icon">⚽</div>
              <div class="team-info">
                <h3 class="team-name">{{ m.team }}</h3>
                <div class="meta">
                  <span class="meta-item">{{ m.sport }}</span>
                  <span class="meta-item">📅 {{ m.date }}</span>
                  <span class="meta-item">⏰ {{ m.time }}</span>
                </div>
                <div class="location">📍 {{ m.location }}</div>
              </div>
            </div>

            <div class="match-right">
              <div class="badges">
                <span v-if="m.status === 'upcoming'" class="badge upcoming">Akan Datang</span>
                <span v-if="m.status === 'completed'" class="badge completed">Selesai</span>
                <span v-if="m.status === 'cancelled'" class="badge cancelled">Dibatalkan</span>
                <span v-if="m.outcome === 'win'" class="badge outcome win">Menang</span>
                <span v-if="m.outcome === 'loss'" class="badge outcome loss">Kalah</span>
                <span v-if="m.outcome === 'draw'" class="badge outcome draw">Seri</span>
              </div>

              <div v-if="m.score" class="score">Skor Akhir: <strong>{{ m.score }}</strong></div>

              <div class="match-actions">
                <button v-if="m.status === 'upcoming'" class="btn-chat">Chat</button>
                <button v-if="m.status === 'upcoming'" class="btn-cancel" @click="cancelMatch(m.id)">Batalkan</button>
              </div>

              <div class="scheduled">Dijadwalkan pada: {{ m.scheduledAt }}</div>
            </div>
          </div>
          <div v-if="filtered.length === 0" class="empty">Tidak ada pertandingan yang cocok.</div>
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped>
.page { min-height: 100vh; background: linear-gradient(180deg, #07122a 0%, #4a2030 48%, #ffffff 100%); color: #0b1220; }
.hero { padding: 36px 60px; max-width: 1100px; margin: 0 auto; }
.hero h1 { color: white; font-size: 34px; margin: 0 0 6px 0; }
.hero .subtitle { color: rgba(255,255,255,0.9); }

.history-content { max-width: 1100px; margin: 20px auto; padding: 20px; }
.stats-row { display: grid; grid-template-columns: repeat(7, 1fr); gap: 18px; margin-bottom: 20px; }
.stat-card { background: white; border-radius: 10px; padding: 18px; box-shadow: 0 8px 24px rgba(0,0,0,0.08); display: flex; flex-direction: column; gap: 8px; align-items: flex-start; }
.stat-card .stat-label { color: #6b7280; font-weight: 600; }
.stat-card .stat-value { font-size: 20px; font-weight: 800; }
.stat-card.blue { border-left: 4px solid #2b6ef6; }
.stat-card.green { border-left: 4px solid #16a34a; }
.stat-card.red { border-left: 4px solid #ef4444; }
.stat-card.grey { border-left: 4px solid #6b7280; }
.stat-card.taller { display: flex; align-items: center; justify-content: center; text-align: center; }

.search-bar { background: linear-gradient(180deg,#fff,#fff); border-radius: 12px; padding: 14px; display: flex; gap: 12px; align-items: center; margin-top: 18px; margin-bottom: 20px; border: 1px solid #e6e9ee; }
.search-bar input { flex: 1; padding: 12px 16px; border-radius: 8px; border: 1px solid #e6e9ee; }
.search-bar .filters { display: flex; gap: 12px; }
.search-bar select { padding: 10px 12px; border-radius: 8px; border: 1px solid #e6e9ee; }

.list { display: grid; gap: 18px; }
.match-card { background: white; padding: 18px; border-radius: 12px; border: 1px solid #eef2f7; box-shadow: 0 8px 24px rgba(0,0,0,0.06); display: flex; gap: 16px; }
.match-left { display: flex; gap: 12px; align-items: center; flex: 1; }
.team-icon { width: 56px; height: 56px; border-radius: 12px; background: linear-gradient(135deg,#e11d48,#be123c); display: flex; align-items: center; justify-content: center; color: white; font-size: 22px; }
.team-name { margin: 0 0 8px 0; }
.meta { display: flex; gap: 12px; color: #6b7280; font-size: 13px; }
.location { color: #6b7280; margin-top: 6px; font-size: 14px; }

.match-right { display: flex; flex-direction: column; gap: 8px; align-items: flex-end; width: 220px; }
.badges { display: flex; gap: 8px; flex-wrap: wrap; justify-content: flex-end; }
.badge { padding: 6px 10px; border-radius: 999px; font-weight: 700; font-size: 12px; }
.badge.upcoming { background: #e0f2ff; color: #0369a1; }
.badge.completed { background: #dcfce7; color: #166534; }
.badge.cancelled { background: #fee2e2; color: #9b1c1c; }
.badge.outcome.win { background: #10b981; color: white; }
.badge.outcome.loss { background: #ef4444; color: white; }
.badge.outcome.draw { background: #6b7280; color: white; }

.score { color: #374151; font-weight: 700; }
.match-actions { display: flex; gap: 8px; }
.btn-chat { background: #2563eb; color: white; border: none; padding: 8px 12px; border-radius: 8px; cursor: pointer; }
.btn-cancel { background: #ef4444; color: white; border: none; padding: 8px 12px; border-radius: 8px; cursor: pointer; }
.scheduled { color: #6b7280; font-size: 13px; }
.empty { text-align: center; color: #6b7280; padding: 24px; }

@media (max-width: 900px) {
  .stats-row { grid-template-columns: repeat(3, 1fr); }
  .match-right { width: 160px; }
}

@media (max-width: 560px) {
  .stats-row { grid-template-columns: repeat(2, 1fr); }
  .hero { padding: 26px 20px; }
  .history-content { padding: 12px; }
  .match-card { flex-direction: column; }
  .match-right { align-items: flex-start; width: 100%; }
}
</style>