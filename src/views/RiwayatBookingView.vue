<script setup lang="ts">
import AppNavbar from "../components/AppNavbar.vue";
import { ref, computed } from 'vue';

// Sample booking data
const bookings = ref([
  {
    id: 1,
    venue: 'Lapangan Futsal Arena Sport',
    location: 'Jakarta Selatan',
    date: '5 Januari 2025',
    time: '18:00 - 20:00',
    type: 'Futsal',
    duration: 2,
    price: 300000,
    bookedAt: '20 Desember 2024',
    status: 'upcoming'
  },
  {
    id: 2,
    venue: 'GOR Basket Senayan',
    location: 'Jakarta Pusat',
    date: '28 Desember 2024',
    time: '14:00 - 16:00',
    type: 'Basket',
    duration: 2,
    price: 400000,
    bookedAt: '15 Desember 2024',
    status: 'completed'
  },
  {
    id: 3,
    venue: 'Lapangan Voli Pantai BSD',
    location: 'Tangerang Selatan',
    date: '22 Desember 2024',
    time: '08:00 - 10:00',
    type: 'Voli',
    duration: 2,
    price: 250000,
    bookedAt: '10 Desember 2024',
    status: 'cancelled'
  },
  {
    id: 4,
    venue: 'Futsal Corner Bekasi',
    location: 'Bekasi',
    date: '10 Januari 2025',
    time: '19:00 - 21:00',
    type: 'Futsal',
    duration: 2,
    price: 280000,
    bookedAt: '23 Desember 2024',
    status: 'upcoming'
  },
  {
    id: 5,
    venue: 'Tennis Court Senopati',
    location: 'Jakarta Selatan',
    date: '25 Desember 2024',
    time: '16:00 - 18:00',
    type: 'Tenis',
    duration: 2,
    price: 350000,
    bookedAt: '18 Desember 2024',
    status: 'completed'
  }
]);

const query = ref('');
const filterStatus = ref('all');

const filtered = computed(() => {
  return bookings.value.filter(b => {
    const matchesQuery = [b.venue, b.location, b.type].join(' ').toLowerCase().includes(query.value.toLowerCase());
    const matchesFilter = filterStatus.value === 'all' ? true : b.status === filterStatus.value;
    return matchesQuery && matchesFilter;
  });
});

const counts = computed(() => ({
  total: bookings.value.length,
  upcoming: bookings.value.filter(b => b.status === 'upcoming').length,
  completed: bookings.value.filter(b => b.status === 'completed').length,
  cancelled: bookings.value.filter(b => b.status === 'cancelled').length
}));

function cancelBooking(id: number) {
  const idx = bookings.value.findIndex(b => b.id === id);
  if (idx !== -1) {
    bookings.value[idx].status = 'cancelled';
  }
}
</script>

<template>
  <div class="page">
    <AppNavbar />
    <main class="history-page">
      <div class="hero">
        <h1>Riwayat Pemesanan Lapangan</h1>
        <p class="subtitle">Kelola semua pemesanan lapangan olahraga Anda</p>
      </div>

      <div class="history-content">
        <div class="stats-row">
          <div class="stat-card">
            <div class="stat-label">Total Pemesanan</div>
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
        </div>

        <div class="search-bar">
          <input type="text" placeholder="Cari lapangan, lokasi, atau jenis olahraga..." v-model="query" />
          <div class="filters">
            <select v-model="filterStatus">
              <option value="all">Semua</option>
              <option value="upcoming">Akan Datang</option>
              <option value="completed">Selesai</option>
              <option value="cancelled">Dibatalkan</option>
            </select>
          </div>
        </div>

        <div class="list">
          <div v-for="b in filtered" :key="b.id" class="booking-card">
            <div class="booking-header">
              <h3 class="venue">{{ b.venue }}</h3>
              <div class="status" :class="b.status">{{ b.status === 'completed' ? 'Selesai' : b.status === 'upcoming' ? 'Akan Datang' : 'Dibatalkan' }}</div>
            </div>

            <div class="booking-meta">
              <div class="meta-item">📍 {{ b.location }}</div>
              <div class="meta-item">📅 {{ b.date }}</div>
              <div class="meta-item">⏰ {{ b.time }}</div>
            </div>

            <div class="booking-details">
              <div>Jenis Lapangan: <strong>{{ b.type }}</strong></div>
              <div>Durasi: <strong>{{ b.duration }} jam</strong></div>
              <div>Total Harga: <strong class="price">Rp {{ b.price.toLocaleString('id-ID') }}</strong></div>
            </div>

            <div class="booking-footer">
              <div class="booked-at">Dibooking pada: {{ b.bookedAt }}</div>
              <div class="actions">
                <button v-if="b.status === 'upcoming'" class="btn-cancel" @click="cancelBooking(b.id)">Batalkan Pemesanan</button>
              </div>
            </div>
          </div>

          <div v-if="filtered.length === 0" class="empty">Tidak ada pesanan yang cocok.</div>
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

.history-page { padding-top: 8px; }
.history-content { max-width: 1100px; margin: 20px auto; padding: 20px; }
.stats-row { display: grid; grid-template-columns: repeat(4, 1fr); gap: 18px; margin-bottom: 20px; }
.stat-card { background: white; border-radius: 10px; padding: 20px; box-shadow: 0 8px 24px rgba(0,0,0,0.08); display: flex; flex-direction: column; gap: 8px; }
.stat-card .stat-label { color: #6b7280; font-weight: 600; }
.stat-card .stat-value { font-size: 20px; font-weight: 800; }
.stat-card.blue { border-left: 4px solid #2b6ef6; }
.stat-card.green { border-left: 4px solid #16a34a; }
.stat-card.red { border-left: 4px solid #ef4444; }

.search-bar { background: linear-gradient(180deg,#fff,#fff); border-radius: 12px; padding: 14px; display: flex; gap: 12px; align-items: center; margin-top: 18px; margin-bottom: 20px; border: 1px solid #e6e9ee; }
.search-bar input { flex: 1; padding: 12px 16px; border-radius: 8px; border: 1px solid #e6e9ee; }
.search-bar select { padding: 10px 12px; border-radius: 8px; border: 1px solid #e6e9ee; }

.list { display: grid; gap: 18px; }
.booking-card { background: white; padding: 18px; border-radius: 12px; border: 1px solid #eef2f7; box-shadow: 0 8px 24px rgba(0,0,0,0.06); }
.booking-header { display: flex; justify-content: space-between; align-items: center; gap: 12px; }
.venue { margin: 0 0 8px 0; }
.status { padding: 6px 10px; border-radius: 20px; font-weight: 700; font-size: 13px; }
.status.upcoming { background: #e0f2ff; color: #0369a1; }
.status.completed { background: #dcfce7; color: #166534; }
.status.cancelled { background: #fee2e2; color: #9b1c1c; }

.booking-meta { display: flex; gap: 14px; color: #6b7280; margin: 8px 0 12px 0; }
.booking-details { display: flex; gap: 16px; color: #374151; align-items: center; padding: 10px 0; border-top: 1px solid #f3f4f6; }
.price { color: #ef4444; }
.booking-footer { display: flex; justify-content: space-between; align-items: center; margin-top: 12px; }
.btn-cancel { padding: 8px 12px; background: #ef4444; color: white; border: none; border-radius: 8px; cursor: pointer; }

.empty { text-align: center; color: #6b7280; padding: 24px; }

@media (max-width: 900px) {
  .stats-row { grid-template-columns: repeat(2, 1fr); }
}

@media (max-width: 560px) {
  .stats-row { grid-template-columns: 1fr; }
  .hero { padding: 26px 20px; }
  .history-content { padding: 12px; }
}
</style>