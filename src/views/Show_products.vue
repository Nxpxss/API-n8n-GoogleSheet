<template>
    <div class="container py-5">
  
      <div class="text-center mb-4">
        <h2 class="fw-bold text-primary">📋 ระบบสต็อกสินค้า</h2>
        <p class="text-muted">ข้อมูลจาก n8n Webhook API</p>
      </div>
  
      <div class="card shadow-lg border-0 rounded-4">
        <div class="card-body">
  
          <div class="d-flex justify-content-between align-items-center mb-3">
            <h5 class="mb-0">รายการข้อมูล</h5>
            <button class="btn btn-primary" @click="fetchData">
              🔄 โหลดข้อมูล
            </button>
          </div>
  
          <div v-if="loading" class="text-center my-4">
            <div class="spinner-border text-primary"></div>
            <p class="mt-2">กำลังโหลดข้อมูล...</p>
          </div>
  
          <div class="table-responsive" v-if="products.length && !loading">
            <table class="table table-hover align-middle text-center">
              <thead class="table-primary">
                <tr>
                  <th>No.</th>
                  <th>รหัสสินค้า</th>
                  <th>ชื่อสินค้า</th>
                  <th>จำนวน</th>
                  <th>ราคา</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(item, index) in products" :key="index">
                  <td>{{ index + 1 }}</td>
                  <td>{{ item.id || item.id }}</td>
                  <td>{{ item.title || item.title }}</td>
                  <td>{{ item.amount }}</td>
                  <td>{{ item.price }}</td>
                </tr>
              </tbody>
            </table>
          </div>
  
          <div v-else-if="!loading" class="text-center text-muted py-4">
            ❌ ไม่มีข้อมูล
          </div>
  
        </div>
      </div>
  
    </div>
</template>
  
<script setup>
  import { ref, onMounted } from 'vue'
  
  const products = ref([])
  const loading = ref(false)
  
  // ข้อมูลจำลองตามรูปภาพที่ให้มา
  const mockData = [
    { id: 'P001', title: 'สมุดปกแข็ง', amount: 20, price: 50 },
    { id: 'P002', title: 'ยางลบ', amount: 24, price: 10 },
    { id: 'P003', title: 'ไม้บรรทัด', amount: 10, price: 15 },
    { id: 'P004', title: 'กระเป๋าดินสอ', amount: 15, price: 99 },
    { id: 'P005', title: 'ปากกาน้ำเงิน', amount: 24, price: 25 },
  ]
  
  const fetchData = async () => {
    loading.value = true
    try {
      // พยายามดึงข้อมูลจาก API
      const response = await fetch('http://localhost:5678/webhook/show')
      if (!response.ok) throw new Error('Network response was not ok')
      const data = await response.json()
      products.value = data
    } catch (error) {
      console.error('Error fetching API, using mock data instead:', error)
      // ถ้า API มีปัญหา ให้ใช้ข้อมูลจำลองจากในรูปแทน เพื่อให้หน้าเว็บทำงานต่อได้
      products.value = mockData
    }
    loading.value = false
  }
  
  onMounted(() => {
    fetchData()
  })
  </script>
  
  <style>
  body {
    background-color: #f8f9fa;
  }
  </style>