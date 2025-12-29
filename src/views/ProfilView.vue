<script setup lang="ts">
import AppNavbar from "../components/AppNavbar.vue";
import { ref } from "vue";

// Data statistik
const stats = ref({
  totalMatches: 45,
  totalWins: 32,
  teamMembers: 15,
  upcomingMatches: 3,
  winRate: "67%",
  teamJoined: 36
});

// Data profil pengguna
const userProfile = ref({
  name: "Ahmad Rizki",
  fullName: "A. Nima Urajigo",
  email: "ahmad.rizki@sportfw.com",
  phone: "+62 812-3456-7800",
  location: "Jakarta Sebbara, Diti Jakarta",
  birthDate: "15 Met 1995",
  bio: "Parma facial embankar yang siwa kierunku et aler paketu Bengaloung dengan SPORTYE untuk menemutan Islamic dan lapangar yang tepak",
  interests: ["Fiscal", "Web"],
  memberSince: "15 Januari 2024",
  memberDays: 714
});

// Data pencapaian
const achievements = ref([
  {
    id: 1,
    title: "Pemain Aktif",
    description: "Menyaletalkan 10 pretradingan",
    icon: "🏃‍♂️"
  },
  {
    id: 2,
    title: "Booker Setia",
    description: "Malakkun 15 booking lapangan",
    icon: "📅"
  }
]);

// Data riwayat booking
const bookingHistory = ref([
  {
    id: 1,
    venue: "Venue Basketball (FINNS)",
    date: "14 November 2024",
    status: "completed",
    distance: "2 km dari lokasi"
  },
  {
    id: 2,
    venue: "Arena Futsal Jakarta Timur",
    date: "15 November 2024",
    status: "pending",
    distance: "5 km dari lokasi"
  },
  {
    id: 3,
    venue: "Pioneer Futsal (FINNS)",
    date: "12 November 2024",
    status: "completed",
    distance: "1 km dari lokasi"
  }
]);

// Edit profile state and functions
const isEditing = ref(false);
const avatarPreview = ref<string | null>(userProfile.value.avatarUrl ?? null);

const editForm = ref({
  name: userProfile.value.name,
  fullName: userProfile.value.fullName,
  email: userProfile.value.email,
  phone: userProfile.value.phone,
  location: userProfile.value.location,
  birthDate: userProfile.value.birthDate,
  bio: userProfile.value.bio,
  interestsText: userProfile.value.interests.join(', '),
  avatarFile: null as File | null
});

function startEdit() {
  editForm.value = {
    name: userProfile.value.name,
    fullName: userProfile.value.fullName,
    email: userProfile.value.email,
    phone: userProfile.value.phone,
    location: userProfile.value.location,
    birthDate: userProfile.value.birthDate,
    bio: userProfile.value.bio,
    interestsText: userProfile.value.interests.join(', '),
    avatarFile: null
  };
  avatarPreview.value = userProfile.value.avatarUrl ?? null;
  isEditing.value = true;
}

function cancelEdit() {
  isEditing.value = false;
  avatarPreview.value = userProfile.value.avatarUrl ?? null;
}

function onAvatarChange(e: Event) {
  const file = (e.target as HTMLInputElement).files?.[0] ?? null;
  if (file) {
    editForm.value.avatarFile = file;
    const reader = new FileReader();
    reader.onload = (ev) => {
      avatarPreview.value = ev.target?.result as string;
    };
    reader.readAsDataURL(file);
  }
}

function saveEdit() {
  // basic validation
  if (!editForm.value.name || !editForm.value.email) {
    alert('Nama dan email wajib diisi');
    return;
  }

  const interestsArr = editForm.value.interestsText
    .split(',')
    .map((s: string) => s.trim())
    .filter(Boolean);

  userProfile.value = {
    ...userProfile.value,
    name: editForm.value.name,
    fullName: editForm.value.fullName,
    email: editForm.value.email,
    phone: editForm.value.phone,
    location: editForm.value.location,
    birthDate: editForm.value.birthDate,
    bio: editForm.value.bio,
    interests: interestsArr,
    avatarUrl: avatarPreview.value ?? userProfile.value.avatarUrl
  };

  isEditing.value = false;
} 

// Fungsi untuk melihat semua riwayat
function viewAllHistory() {
  alert("Menampilkan semua riwayat booking dan pertandingan");
}
</script>

<template>
  <div class="page">
    <!-- NAVBAR -->
    <AppNavbar />

    <!-- PROFILE CONTENT -->
    <main class="dashboard">
      <div class="hero">
        <div class="dashboard-header">
          <h1>Profil Pengguna</h1>
          <p class="subtitle">Kelola informasi pribadi dan preferensi Anda</p>
        </div>
      </div>

      <div class="main-content">
        <!-- KOLOM KIRI - INFO PROFIL -->
        <div class="left-column">
          <!-- KARTU PROFIL -->
          <div class="profile-card">
            <div class="profile-header">
              <button class="edit-btn profile-edit" @click="startEdit">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"></path>
                  <path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"></path>
                </svg>
                Edit Profil
              </button> 

              <div class="avatar-section">
                <div class="avatar">
                  <template v-if="userProfile.avatarUrl">
                    <img :src="userProfile.avatarUrl" alt="Avatar" class="avatar-img" />
                  </template>
                  <template v-else>
                    <span class="avatar-letter">{{ userProfile.name ? userProfile.name.charAt(0).toUpperCase() : 'A' }}</span>
                  </template>

                  <div class="camera-overlay" title="Ubah Foto">
                    <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M23 19a2 2 0 0 1-2 2H3a2 2 0 0 1-2-2V7a2 2 0 0 1 2-2h3l2-3h6l2 3h3a2 2 0 0 1 2 2z"></path><circle cx="12" cy="13" r="3"></circle></svg>
                  </div>
                </div>
                <div class="name-section">
                  <div class="label-full">Nama</div>
                  <div class="username">{{ userProfile.name }}</div>
                </div>
              </div>
            </div> 

            <!-- INFO KONTAK -->
            <div v-if="isEditing" class="edit-form">
              <div class="form-row">
                <label>Nama Lengkap</label>
                <input type="text" v-model="editForm.fullName" />
              </div>

              <div class="form-row">
                <label>Nama Pengguna</label>
                <input type="text" v-model="editForm.name" />
              </div>

              <div class="form-row">
                <label>Email</label>
                <input type="email" v-model="editForm.email" />
              </div>

              <div class="form-row">
                <label>Nomor Telepon</label>
                <input type="text" v-model="editForm.phone" />
              </div>

              <div class="form-row">
                <label>Lokasi</label>
                <input type="text" v-model="editForm.location" />
              </div>

              <div class="form-row">
                <label>Tanggal Lahir</label>
                <input type="text" v-model="editForm.birthDate" />
              </div>

              <div class="form-row">
                <label>Bio</label>
                <textarea rows="3" v-model="editForm.bio"></textarea>
              </div>

              <div class="form-row">
                <label>Olahraga Favorit (pisah koma)</label>
                <input type="text" v-model="editForm.interestsText" />
              </div>

              <div class="form-row">
                <label>Foto Profil</label>
                <div class="avatar-upload">
                  <div class="avatar-preview" v-if="avatarPreview"><img :src="avatarPreview" alt="Preview" /></div>
                  <input type="file" accept="image/*" @change="onAvatarChange" />
                </div>
              </div>

              <div class="form-actions">
                <button class="save-btn" @click="saveEdit">Simpan</button>
                <button class="cancel-btn" @click="cancelEdit">Batal</button>
              </div>
            </div>

            <div v-else class="contact-info">
              <div class="info-row">
                <div class="info-label">Email</div>
                <div class="info-value"><svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="#6b7280" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M4 4h16v16H4z"></path><polyline points="22,6 12,13 2,6"></polyline></svg> {{ userProfile.email }}</div>
              </div>
              
              <div class="info-row">
                <div class="info-label">Nomor Telepon</div>
                <div class="info-value"><svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="#6b7280" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6A19.79 19.79 0 0 1 3.08 4.18 2 2 0 0 1 5 2h3a2 2 0 0 1 2 1.72c.12.95.35 1.86.7 2.73a2 2 0 0 1-.45 2.11L9 10.92"></path></svg> {{ userProfile.phone }}</div>
              </div>
              
              <div class="info-row">
                <div class="info-label">Lokasi</div>
                <div class="info-value"><svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="#6b7280" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M21 10c0 6-9 12-9 12S3 16 3 10a9 9 0 1 1 18 0z"></path><circle cx="12" cy="10" r="3"></circle></svg> {{ userProfile.location }}</div>
              </div>
              
              <div class="info-row">
                <div class="info-label">Tanggal Lahir</div>
                <div class="info-value"><svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="#6b7280" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect><line x1="16" y1="2" x2="16" y2="6"></line><line x1="8" y1="2" x2="8" y2="6"></line><line x1="3" y1="10" x2="21" y2="10"></line></svg> {{ userProfile.birthDate }}</div>
              </div>
            </div>

            <!-- BIO -->
            <div class="bio-section">
              <div class="bio-label">Bio</div>
              <div class="bio-text">{{ userProfile.bio }}</div>
            </div>

            <!-- MINAT -->
            <div class="interests-section">
              <div class="interests-label">Olahraga Favorit</div>
              <div class="tags">
                <span 
                  v-for="(interest, index) in userProfile.interests" 
                  :key="index" 
                  class="tag"
                >
                  {{ interest }}
                </span>
              </div>
            </div>
          </div>
        </div>

        <!-- KOLOM KANAN - STATISTIK & INFORMASI -->
        <div class="right-column">
          <!-- KARTU STATISTIK -->
          <div class="stats-card">
            <div class="section-header">
              <h2>Statistik</h2>
              <span class="section-subtitle">Task Center</span>
            </div>

            <!-- TOMBOL EDIT PROFIL -->


            <!-- GRID STATISTIK -->
            <div class="stats-grid">
              <div class="stat-box">
                <div class="stat-label">Total Pertandingan</div>
                <div class="stat-value">{{ stats.totalMatches }}</div>
              </div>
              <div class="stat-box">
                <div class="stat-label">Total Existing</div>
                <div class="stat-value">{{ stats.upcomingMatches }}</div>
              </div>
              <div class="stat-box full-width">
                <div class="stat-label">Win Rate</div>
                <div class="stat-value-large">{{ stats.winRate }}</div>
              </div>
              <div class="stat-box full-width">
                <div class="stat-label">Tim Semnan</div>
                <div class="stat-value-large">{{ stats.teamJoined }}</div>
              </div>
            </div>
          </div>

          <!-- KARTU INFORMASI AKUN -->
          <div class="account-card">
            <div class="section-header">
              <h2>Informasi Akun</h2>
            </div>
            <div class="account-details">
              <div class="account-item">
                <div class="account-label">Regulatory Split</div>
                <div class="account-value">{{ userProfile.memberSince }}</div>
              </div>
              <div class="account-item">
                <div class="account-label">Member Duration</div>
                <div class="account-value">{{ userProfile.memberDays }} hari</div>
              </div>
            </div>
          </div>

          <!-- KARTU PENCAPAIAN -->
          <div class="achievements-card">
            <div class="section-header">
              <h2>Pencapalan</h2>
            </div>
            
            <!-- AKSI CEPAT -->
            <div class="quick-actions">
              <h4 class="quick-actions-title">Aksi Cepat</h4>
              <div class="quick-actions-buttons">
                <button class="quick-action-btn">Booking Baru</button>
                <button class="quick-action-btn">Lihat Jadwal</button>
                <button class="quick-action-btn">Undang Teman</button>
              </div>
            </div>

            <!-- DAFTAR PENCAPAIAN -->
            <div class="achievements-list">
              <div class="achievements-grid">
                <div v-for="achievement in achievements" :key="achievement.id" class="achievement-card">
                  <div class="achievement-top">
                    <div class="achievement-icon">{{ achievement.icon }}</div>
                  </div>
                  <div class="achievement-body">
                    <h5 class="achievement-title">{{ achievement.title }}</h5>
                    <p class="achievement-desc">{{ achievement.description }}</p>
                  </div>
                </div>
              </div>
            </div>

            <!-- RIWAYAT BOOKING -->
            <div class="history-section">
              <div class="history-tabs">
                <button class="history-tab active">Kikayat Booking</button>
                <button class="history-tab">Kikayat Pretradingan</button>
              </div>
              
              <div class="history-content">
                <div 
                  v-for="booking in bookingHistory" 
                  :key="booking.id" 
                  class="history-item"
                >
                  <div class="history-date">{{ booking.date }}</div>
                  <div class="history-desc">{{ booking.venue }}</div>
                  <div class="history-distance">{{ booking.distance }}</div>
                  <div :class="`history-status ${booking.status}`">
                    {{ booking.status === 'completed' ? 'Selesai' : 'Pending' }}
                  </div>
                </div>
              </div>
              
              <button class="view-all-btn" @click="viewAllHistory">
                Lihat Semua →
              </button>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped>
.page {
  min-height: 100vh;
  font-family: "Segoe UI", system-ui, -apple-system, Arial, sans-serif;
  color: #111827;
  background: linear-gradient(180deg, #07122a 0%, #4a2030 48%, #ffffff 100%);
  background-attachment: fixed;
}

.hero {
  background: linear-gradient(90deg, #07122a 0%, #2b1b3a 100%);
  color: white;
  padding: 42px 60px;
  border-bottom-left-radius: 12px;
  border-bottom-right-radius: 12px;
  margin-bottom: 24px;
}

.hero .dashboard-header h1 {
  color: #ffffff;
  font-size: 34px;
  margin: 0 0 6px 0;
}

.hero .subtitle {
  color: rgba(255,255,255,0.85);
}

/* DASHBOARD */
.dashboard {
  padding: 0 60px 60px 60px;
  max-width: 1100px;
  margin: 0 auto;
}

.dashboard-header {
  margin-bottom: 40px;
}

.dashboard-header h1 {
  font-size: 32px;
  font-weight: 700;
  color: #111827;
  line-height: 1.3;
  margin-bottom: 8px;
}

.subtitle {
  color: #6b7280;
  font-size: 16px;
  margin: 0;
}

/* MAIN CONTENT LAYOUT */
.main-content {
  display: grid;
  grid-template-columns: 1fr;
  gap: 28px;
  max-width: 900px;
  margin: 0 auto;
} 

/* LEFT COLUMN STYLES */
.left-column {
  background: white;
  border-radius: 16px;
  padding: 32px;
  box-shadow: 0 8px 40px rgba(0, 0, 0, 0.12);
  border: 1px solid rgba(0,0,0,0.04);
  height: fit-content;
}

.profile-card {
  background: white;
  border-radius: 14px;
  padding: 36px;
  box-shadow: 0 12px 40px rgba(0,0,0,0.12);
  border: 1px solid #edf2f7;
  position: relative;
  margin-top: -40px; /* overlap hero */
}

.profile-edit {
  position: absolute;
  top: 20px;
  right: 20px;
  padding: 8px 14px;
  border-radius: 10px;
  font-size: 14px;
}

.avatar {
  width: 96px;
  height: 96px;
  border-radius: 999px;
  background: linear-gradient(135deg, #e63946 0%, #ff6b6b 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 36px;
  font-weight: 700;
  position: relative;
  box-shadow: 0 8px 20px rgba(0,0,0,0.12);
}

.camera-overlay {
  position: absolute;
  right: -6px;
  bottom: -6px;
  width: 32px;
  height: 32px;
  border-radius: 999px;
  background: white;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 6px 18px rgba(0,0,0,0.12);
}

.label-full {
  font-size: 13px;
  color: #9ca3af;
  font-weight: 600;
  margin-bottom: 6px;
}

.username {
  font-size: 28px;
  font-weight: 800;
  color: #0b1220;
}

.tags .tag {
  background: linear-gradient(135deg, #e63946 0%, #ff6b6b 100%);
  color: white;
  padding: 8px 14px;
  border-radius: 18px;
  font-size: 13px;
  font-weight: 700;
}

.achievements-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 16px;
}

.achievement-card {
  display: flex;
  gap: 12px;
  align-items: center;
  background: white;
  border-radius: 12px;
  padding: 16px;
  border: 1px solid #eef2f7;
  box-shadow: 0 6px 18px rgba(0,0,0,0.06);
}

.achievement-top .achievement-icon {
  font-size: 20px;
}

.achievement-body .achievement-title {
  font-size: 15px;
  font-weight: 700;
}

.achievement-body .achievement-desc {
  font-size: 13px;
  color: #6b7280;
}

.profile-header {
  margin-bottom: 32px;
  padding-bottom: 24px;
  border-bottom: 1px solid #e5e7eb;
}

.avatar-section {
  display: flex;
  align-items: center;
  gap: 20px;
}

.avatar {
  width: 72px;
  height: 72px;
  background: linear-gradient(135deg, #e63946 0%, #ff6b6b 100%);
  border-radius: 16px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  font-size: 20px;
  font-weight: 600;
  color: white;
  line-height: 1.2;
  flex-shrink: 0;
}

.name-section {
  flex: 1;
}

.full-name {
  font-size: 14px;
  color: #6b7280;
  margin-bottom: 4px;
  font-weight: 500;
}

.username {
  font-size: 28px;
  font-weight: 700;
  color: #111827;
  line-height: 1.2;
}

/* CONTACT INFO */
.contact-info {
  margin-bottom: 32px;
}

.info-row {
  display: flex;
  margin-bottom: 20px;
  padding-bottom: 12px;
  border-bottom: 1px solid #f3f4f6;
}

.info-row:last-child {
  margin-bottom: 0;
  border-bottom: none;
}

.info-label {
  flex: 0 0 140px;
  font-size: 14px;
  color: #6b7280;
  font-weight: 500;
}

.info-value {
  flex: 1;
  font-size: 16px;
  color: #374151;
  font-weight: 500;
}

/* BIO SECTION */
.bio-section {
  margin-bottom: 32px;
  padding-bottom: 24px;
  border-bottom: 1px solid #e5e7eb;
}

/* EDIT FORM */
.edit-form {
  display: flex;
  flex-direction: column;
  gap: 12px;
  margin-bottom: 20px;
}

.form-row {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.form-row label {
  font-size: 13px;
  color: #6b7280;
  font-weight: 600;
}

.edit-form input[type="text"],
.edit-form input[type="email"],
.edit-form textarea,
.edit-form input[type="file"] {
  padding: 10px 12px;
  border-radius: 8px;
  border: 1px solid #e6e9ee;
  font-size: 14px;
}

.edit-form textarea { resize: vertical; }

.avatar-upload { display: flex; align-items: center; gap: 12px; }
.avatar-preview img { width: 64px; height: 64px; border-radius: 999px; object-fit: cover; box-shadow: 0 6px 18px rgba(0,0,0,0.12); }

.form-actions { display: flex; gap: 12px; justify-content: flex-end; margin-top: 8px; }
.save-btn { padding: 10px 18px; background: linear-gradient(135deg, #e63946 0%, #ff6b6b 100%); color: white; border: none; border-radius: 8px; font-weight: 700; cursor: pointer; }
.cancel-btn { padding: 8px 14px; background: #fff; color: #374151; border: 1px solid #e6e9ee; border-radius: 8px; cursor: pointer; }

/* avatar image */
.avatar-img { width: 100%; height: 100%; border-radius: inherit; object-fit: cover; }


.bio-label {
  font-size: 14px;
  color: #6b7280;
  font-weight: 500;
  margin-bottom: 12px;
}

.bio-text {
  font-size: 15px;
  line-height: 1.6;
  color: #374151;
}

/* INTERESTS SECTION */
.interests-section {
  margin-top: 24px;
}

.interests-label {
  font-size: 14px;
  color: #6b7280;
  font-weight: 500;
  margin-bottom: 12px;
}

.tags {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
}

.tag {
  background: linear-gradient(135deg, #e63946 0%, #ff6b6b 100%);
  color: white;
  padding: 10px 20px;
  border-radius: 24px;
  font-size: 14px;
  font-weight: 600;
}

/* RIGHT COLUMN STYLES */
.right-column {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

/* SECTION HEADER COMMON STYLES */
.section-header {
  margin-bottom: 24px;
}

.section-header h2 {
  font-size: 24px;
  font-weight: 700;
  color: #111827;
  margin-bottom: 6px;
}

.section-subtitle {
  color: #6b7280;
  font-size: 14px;
  font-weight: 500;
}

/* STATS CARD */
.stats-card {
  background: white;
  border-radius: 16px;
  padding: 32px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
  border: 1px solid #e5e7eb;
}

.stats-header {
  display: flex;
  justify-content: flex-end;
  margin-bottom: 24px;
  padding-bottom: 20px;
  border-bottom: 1px solid #e5e7eb;
}

.edit-btn {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 10px 20px;
  background: linear-gradient(135deg, #e63946 0%, #ff6b6b 100%);
  color: white;
  border: none;
  border-radius: 10px;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.edit-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(230, 57, 70, 0.3);
}

/* STATS GRID */
.stats-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 24px;
}

.stat-box {
  background: #f8fafc;
  border: 1px solid #e5e7eb;
  border-radius: 12px;
  padding: 20px;
  text-align: center;
  transition: all 0.3s ease;
}

.stat-box:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
  border-color: #d1d5db;
}

.stat-box.full-width {
  grid-column: span 2;
}

.stat-label {
  font-size: 14px;
  color: #6b7280;
  margin-bottom: 8px;
  font-weight: 500;
}

.stat-value {
  font-size: 32px;
  font-weight: 800;
  color: #111827;
}

.stat-value-large {
  font-size: 48px;
  font-weight: 800;
  color: #111827;
  line-height: 1;
}

/* ACCOUNT CARD */
.account-card {
  background: white;
  border-radius: 16px;
  padding: 32px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
  border: 1px solid #e5e7eb;
}

.account-details {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.account-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 0;
  border-bottom: 1px solid #f3f4f6;
}

.account-item:last-child {
  border-bottom: none;
}

.account-label {
  font-size: 15px;
  color: #6b7280;
  font-weight: 500;
}

.account-value {
  font-size: 16px;
  color: #111827;
  font-weight: 600;
}

/* ACHIEVEMENTS CARD */
.achievements-card {
  background: white;
  border-radius: 16px;
  padding: 32px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
  border: 1px solid #e5e7eb;
}

/* QUICK ACTIONS */
.quick-actions {
  margin-bottom: 24px;
  padding-bottom: 20px;
  border-bottom: 1px solid #e5e7eb;
}

.quick-actions-title {
  font-size: 18px;
  font-weight: 600;
  color: #374151;
  margin-bottom: 16px;
}

.quick-actions-buttons {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
}

.quick-action-btn {
  padding: 10px 20px;
  background: #f8fafc;
  color: #374151;
  border: 1px solid #d1d5db;
  border-radius: 10px;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.quick-action-btn:hover {
  background: #e5e7eb;
  transform: translateY(-2px);
}

/* ACHIEVEMENTS LIST */
.achievements-list {
  margin-bottom: 28px;
}

.achievement-item {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 16px;
  background: #f8fafc;
  border-radius: 12px;
  margin-bottom: 12px;
  border: 1px solid #e5e7eb;
  transition: all 0.3s ease;
}

.achievement-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  border-color: #d1d5db;
}

.achievement-item:last-child {
  margin-bottom: 0;
}

.achievement-icon {
  font-size: 24px;
  flex-shrink: 0;
}

.achievement-content {
  flex: 1;
}

.achievement-title {
  font-size: 16px;
  font-weight: 600;
  color: #111827;
  margin-bottom: 4px;
}

.achievement-desc {
  font-size: 14px;
  color: #6b7280;
  line-height: 1.4;
}

/* HISTORY SECTION */
.history-section {
  background: #f8fafc;
  border-radius: 12px;
  padding: 20px;
  border: 1px solid #e5e7eb;
}

.history-tabs {
  display: flex;
  gap: 8px;
  margin-bottom: 20px;
  border-bottom: 1px solid #e5e7eb;
  padding-bottom: 12px;
}

.history-tab {
  padding: 10px 20px;
  background: none;
  border: none;
  font-size: 14px;
  font-weight: 500;
  color: #6b7280;
  cursor: pointer;
  border-radius: 8px;
  transition: all 0.3s ease;
}

.history-tab.active {
  background: linear-gradient(135deg, #e63946 0%, #ff6b6b 100%);
  color: white;
}

.history-tab:hover:not(.active) {
  background: #e5e7eb;
}

.history-content {
  margin-bottom: 20px;
}

.history-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 14px;
  background: white;
  border-radius: 10px;
  margin-bottom: 10px;
  border: 1px solid #e5e7eb;
  transition: all 0.3s ease;
}

.history-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.history-date {
  font-size: 14px;
  color: #6b7280;
  font-weight: 500;
  min-width: 120px;
}

.history-desc {
  flex: 1;
  font-size: 15px;
  color: #374151;
  font-weight: 500;
}

.history-distance {
  font-size: 12px;
  color: #6b7280;
  font-weight: 500;
  margin: 0 12px;
  padding: 4px 10px;
  background: #e0e7ff;
  border-radius: 20px;
}

.history-status {
  padding: 6px 12px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 600;
}

.history-status.completed {
  background: #dcfce7;
  color: #166534;
}

.history-status.pending {
  background: #fef3c7;
  color: #92400e;
}

/* VIEW ALL BUTTON */
.view-all-btn {
  width: 100%;
  padding: 12px;
  background: none;
  border: 1px solid #d1d5db;
  border-radius: 10px;
  font-size: 15px;
  font-weight: 600;
  color: #374151;
  cursor: pointer;
  transition: all 0.3s ease;
  text-align: center;
}

.view-all-btn:hover {
  background: #f3f4f6;
  border-color: #9ca3af;
  transform: translateY(-2px);
}

/* RESPONSIVE DESIGN */
@media (max-width: 1200px) {
  .main-content {
    grid-template-columns: 1fr;
  }
  
  .left-column,
  .right-column {
    min-width: 100%;
  }
}

@media (max-width: 1024px) {
  .dashboard {
    padding: 30px;
  }
}

@media (max-width: 768px) {
  .dashboard {
    padding: 20px;
  }
  
  .dashboard-header h1 {
    font-size: 24px;
  }
  
  .avatar-section {
    flex-direction: column;
    text-align: center;
    gap: 16px;
  }
  
  .info-row {
    flex-direction: column;
    gap: 4px;
  }
  
  .info-label {
    flex: none;
  }
  
  .stats-grid {
    grid-template-columns: 1fr;
  }
  
  .stat-box.full-width {
    grid-column: span 1;
  }
  
  .account-item {
    flex-direction: column;
    align-items: flex-start;
    gap: 4px;
  }
  
  .history-item {
    flex-direction: column;
    align-items: flex-start;
    gap: 8px;
  }
  
  .history-distance,
  .history-status {
    align-self: flex-start;
  }
  
  .quick-actions-buttons {
    flex-direction: column;
  }
}

@media (max-width: 480px) {
  .tags {
    flex-direction: column;
  }
  
  .tag {
    width: 100%;
    text-align: center;
  }
  
  .history-tabs {
    flex-direction: column;
  }
}
</style>