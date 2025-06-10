<template>
  <div class="min-h-screen bg-[#0D0D0D] text-white p-8">
    <!-- Header -->
    <header class="text-center mb-8">
      <h1 class="text-3xl font-extrabold text-[#fc9414]">Galeria Sophilia Ticketing</h1>
    </header>

    <!-- Form Input -->
    <div class="bg-white text-black p-6 rounded-xl max-w-3xl mx-auto">
      <h2 class="text-xl font-semibold text-[#fc9414] mb-4">Input Ticket</h2>
      <div class="grid grid-cols-5 gap-4 items-end">
        <!-- Input G -->
        <div>
          <label class="block font-medium text-sm">G</label>
          <input v-model.number="form.g" type="number" min="0" class="w-full border border-gray-300 p-2 rounded" />
        </div>
        <!-- Input S -->
        <div>
          <label class="block font-medium text-sm">S</label>
          <input v-model.number="form.s" type="number" min="0" class="w-full border border-gray-300 p-2 rounded" />
        </div>
        <!-- Input C -->
        <div>
          <label class="block font-medium text-sm">C</label>
          <input v-model.number="form.c" type="number" min="0" class="w-full border border-gray-300 p-2 rounded" />
        </div>
        <!-- Payment Method -->
        <div>
          <label class="block font-medium text-sm">Metode</label>
          <select v-model="form.method" class="w-full border border-gray-300 p-2 rounded">
            <option>QRIS</option>
            <option>Tunai</option>
            <option>Debit</option>
          </select>
        </div>
        <!-- Country -->
        <div>
          <label class="block font-medium text-sm">Negara</label>
          <select v-model="form.country" class="w-full border border-gray-300 p-2 rounded">
            <option>Indonesia</option>
            <option>Malaysia</option>
            <option>Singapura</option>
            <option>Asing</option>
          </select>
        </div>
      </div>

      <!-- Submit Button -->
      <div class="mt-4 text-center">
        <button @click="addTransaction" class="bg-[#fc9414] text-white px-6 py-2 rounded hover:bg-orange-500 transition">
          Tambah Transaksi
        </button>
      </div>

      <!-- Rangkuman Total Pengunjung & Pemasukan -->
      <div class="mt-6 border-t pt-4 text-sm text-gray-700">
        <p><strong>Total Pengunjung:</strong> {{ totalVisitors }}</p>
        <p><strong>Total Pemasukan:</strong> Rp {{ totalRevenue.toLocaleString() }}</p>
      </div>
    </div>

    <!-- Tabel Transaksi -->
    <div class="mt-10 max-w-4xl mx-auto">
      <h2 class="text-xl font-semibold text-[#fc9414] mb-4">Ringkasan Transaksi</h2>
      <table class="w-full text-left border-separate border-spacing-y-2">
        <thead class="bg-[#fc9414] text-white">
          <tr>
            <th class="p-2">#</th>
            <th>Time</th>
            <th>G</th>
            <th>S</th>
            <th>C</th>
            <th>Metode</th>
            <th>Negara</th>
            <th>Total</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(t, index) in transactions" :key="index" class="bg-white text-black rounded">
            <td class="p-2">{{ index + 1 }}</td>
            <td class="max-w-10">{{ t.time }}</td>
            <td>{{ t.g }}</td>
            <td>{{ t.s }}</td>
            <td>{{ t.c }}</td>
            <td >{{ t.method }}</td>
            <td>{{ t.country }}</td>
            <td>Rp {{ t.total.toLocaleString() }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import { reactive, computed } from 'vue'

const form = reactive({
  g: 0,
  s: 0,
  c: 0,
  method: 'QRIS',
  country: 'Indonesia'
})

const transactions = reactive([])

const addTransaction = () => {
  const total = form.g * 50000 + form.s * 75000 + form.c * 100000
  const time = new Date().toLocaleString('id-ID')
  transactions.push({ ...form, total ,time})
  form.g = form.s = form.c = 0
  form.method = 'QRIS'
  form.country = 'Indonesia'
}

const totalVisitors = computed(() =>
  transactions.reduce((sum, t) => sum + t.g + t.s + t.c, 0)
)

const totalRevenue = computed(() =>
  transactions.reduce((sum, t) => sum + t.total, 0)
)
</script>
