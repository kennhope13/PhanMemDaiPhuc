<script setup>
import { ref, onMounted } from 'vue'
import { 
  Download, 
  Facebook, 
  Linkedin,
  Youtube, 
  Menu, 
  X, 
  Send,
  CheckCircle,
  Monitor,
  Server
} from 'lucide-vue-next'

useHead({
  title: 'Công ty phần mềm Đại Phúc',
  meta: [
    { name: 'description', content: 'Công ty phần mềm Đại Phúc là doanh nghiệp chuyên cung cấp các giải pháp công nghệ thông tin, phát triển ứng dụng và dịch vụ gia công phần mềm.' },
    { property: 'og:title', content: 'Đại Phúc' },
    { property: 'og:description', content: 'Tải phần mềm Station Monitor Setup 3.0.51.exe mới nhất.' }
  ],
  link: [
    { rel: 'preconnect', href: 'https://fonts.googleapis.com' },
    { rel: 'preconnect', href: 'https://fonts.gstatic.com', crossorigin: '' },
    { rel: 'icon', type: 'image/png', href: '/logo.png' }
  ]
})

const isMobileMenuOpen = ref(false)
const activeTab = ref('about')
const isDownloading = ref(false)
const downloadSuccess = ref(false)
const showContactModal = ref(false)

// Dynamic release information with default fallbacks
const stationMonitorInfo = ref({
  version: '3.0.51',
  fileName: 'Station.Monitor.Setup.3.0.51.exe',
  downloadUrl: 'https://github.com/kennhope13/Power-Monitor/releases/download/v3.0.51/Station.Monitor.Setup.3.0.51.exe'
})

const masterStationInfo = ref({
  version: '3.0.10',
  fileName: 'Master.Station.Setup.3.0.10.exe',
  downloadUrl: 'https://github.com/kennhope13/Master-Station/releases/download/v3.0.10/Master.Station.Setup.3.0.10.exe'
})

onMounted(async () => {
  // Fetch Station Monitor latest release
  try {
    const res = await fetch('https://api.github.com/repos/kennhope13/Power-Monitor/releases/latest')
    if (res.ok) {
      const data = await res.json()
      const exeAsset = data.assets.find(asset => asset.name.toLowerCase().endsWith('.exe'))
      if (exeAsset) {
        stationMonitorInfo.value = {
          version: data.tag_name.replace('v', ''),
          fileName: exeAsset.name,
          downloadUrl: exeAsset.browser_download_url
        }
      }
    }
  } catch (err) {
    console.error('Error fetching latest Power-Monitor release:', err)
  }

  // Fetch Master Station latest release
  try {
    const res = await fetch('https://api.github.com/repos/kennhope13/Master-Station/releases/latest')
    if (res.ok) {
      const data = await res.json()
      const exeAsset = data.assets.find(asset => asset.name.toLowerCase().endsWith('.exe'))
      if (exeAsset) {
        masterStationInfo.value = {
          version: data.tag_name.replace('v', ''),
          fileName: exeAsset.name,
          downloadUrl: exeAsset.browser_download_url
        }
      }
    }
  } catch (err) {
    console.error('Error fetching latest Master-Station release:', err)
  }
})

// Contact form state
const formName = ref('')
const formEmail = ref('')
const formMsg = ref('')
const isSubmitting = ref(false)
const submitSuccess = ref(false)

const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value
}

const scrollToSection = (id) => {
  isMobileMenuOpen.value = false
  activeTab.value = id
  
  if (id === 'contact') {
    showContactModal.value = true
    return
  }
  
  const element = document.getElementById(id)
  if (element) {
    const headerOffset = 120
    const elementPosition = element.getBoundingClientRect().top
    const offsetPosition = elementPosition + window.pageYOffset - headerOffset

    window.scrollTo({
      top: offsetPosition,
      behavior: 'smooth'
    })
  }
}

const downloadedFile = ref('')
const handleDownload = (filename) => {
  isDownloading.value = true
  downloadedFile.value = filename
  
  setTimeout(() => {
    isDownloading.value = false
    downloadSuccess.value = true
    
    // Trigger actual download of the installer file
    const link = document.createElement('a')
    link.href = `/${filename}`
    link.download = filename
    document.body.appendChild(link)
    link.click()
    document.body.removeChild(link)
    
    setTimeout(() => {
      downloadSuccess.value = false
    }, 5000)
  }, 1000)
}

const triggerDownloadToast = (filename) => {
  downloadedFile.value = filename
  downloadSuccess.value = true
  setTimeout(() => {
    downloadSuccess.value = false
  }, 5000)
}

const submitContactForm = () => {
  isSubmitting.value = true
  setTimeout(() => {
    isSubmitting.value = false
    submitSuccess.value = true
    formName.value = ''
    formEmail.value = ''
    formMsg.value = ''
    
    setTimeout(() => {
      submitSuccess.value = false
      showContactModal.value = false
    }, 3000)
  }, 1500)
}
</script>

<template>
  <div class="site-root-container">
    <!-- Paper Grain Texture -->
    <div class="paper-grain" />

    <!-- Organic Background Blobs -->
    <div class="bg-blobs-container">
      <!-- Top Left Blob -->
      <svg class="blob blob-1" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
        <path fill="#E2E9DF" d="M45,-76C58,-69,69,-56,76,-41C83,-27,87,-13,85,0C83,14,75,28,68,43C61,57,55,72,43,80C31,88,16,90,1,88C-14,86,-28,81,-42,73C-55,66,-67,56,-75,43C-83,30,-87,15,-87,0C-87,-14,-83,-29,-75,-42C-67,-55,-55,-66,-41,-74C-28,-81,-14,-85,1,-86C15,-87,29,-85,45,-76Z" transform="translate(100 100)" />
      </svg>
      <!-- Top Right Blob -->
      <svg class="blob blob-2" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
        <path fill="#EFE9E2" d="M52.3,-72.5C65.5,-64.1,72.4,-46.8,77.4,-29.4C82.4,-12,85.5,5.6,81.4,21.5C77.4,37.4,66.1,51.6,52.2,61.9C38.3,72.2,21.7,78.7,4.3,72.8C-13.1,66.9,-31.3,48.5,-45.5,38C-59.7,27.5,-69.8,24.8,-75.4,16.5C-81.1,8.3,-82.2,-5.6,-77.9,-18.2C-73.6,-30.8,-63.9,-42.2,-52.1,-51.1C-40.3,-60,-26.4,-66.4,-10.8,-70.7C4.8,-75,21,-77.3,37.1,-75.3C53.2,-73.3,65.5,-64.1,52.3,-72.5Z" transform="translate(100 100)" />
      </svg>
      <!-- Bottom Left Blob -->
      <svg class="blob blob-3" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
        <path fill="#EFE9E2" d="M48.2,-70.1C59.9,-61.7,65.1,-43.3,70.5,-26.2C75.8,-9.1,81.3,6.8,78.7,21.6C76.1,36.5,65.5,50.3,52.4,60.8C39.3,71.2,23.8,78.4,7.4,77.3C-9,76.2,-26.3,66.9,-39.8,56.1C-53.3,45.3,-63,33,-69,18.3C-75.1,3.5,-77.4,-13.7,-72.1,-28.4C-66.9,-43,-54,-55.1,-39.9,-62.7C-25.9,-70.3,-10.7,-73.4,2.9,-77.8C16.5,-82.2,36.4,-78.5,48.2,-70.1Z" transform="translate(100 100)" />
      </svg>
      <!-- Bottom Right Blob -->
      <svg class="blob blob-4" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
        <path fill="#E2E9DF" d="M37.9,-54C47.7,-46.8,53.4,-32.8,57,-18.2C60.6,-3.6,62,11.5,57.1,24.1C52.2,36.7,40.9,46.7,28.2,52.8C15.5,58.8,1.4,60.9,-12.3,58C-26,55.1,-39.4,47.2,-48.8,35.7C-58.2,24.2,-63.7,9.2,-63.9,-6C-64.2,-21.2,-59.2,-36.5,-49.2,-44C-39.2,-51.5,-24.1,-51.2,-10.1,-52C3.9,-52.8,17.9,-54.7,28.2,-55.5C38.5,-56.3,28.1,-61.2,37.9,-54Z" transform="translate(100 100)" />
      </svg>
    </div>



    <!-- Main Content Area -->
    <main class="page-split-layout">
      
      <!-- Top Branding & Navigation -->
      <header class="top-logo-bar">
        <a href="#" class="brand-logo-link" @click.prevent="scrollToSection('about')">
          <img src="/logo.png?v=2" alt="Đại Phúc Logo" class="logo-image-top" />
          <div class="logo-text-group">
            <span class="logo-brand-name">DAI PHUC</span>
            <span class="logo-slogan">Giải pháp tối ưu, Khởi nguyên thịnh vượng</span>
          </div>
        </a>
        
        <nav class="top-nav-menu">
          <button 
            class="top-nav-link" 
            :class="{ active: activeTab === 'about' }"
            @click="scrollToSection('about')"
          >
            Giới thiệu
          </button>
          <button 
            class="top-nav-link" 
            :class="{ active: activeTab === 'product' }"
            @click="scrollToSection('product')"
          >
            Sản phẩm
          </button>
          <button 
            class="top-nav-link" 
            :class="{ active: showContactModal }"
            @click="scrollToSection('contact')"
          >
            Liên hệ
          </button>
        </nav>
      </header>

      <!-- Split Grid Container -->
      <div class="split-grid-container">
        
        <!-- Left Side: Introduction (Giới thiệu) -->
        <section id="about" class="split-left-about">
          <h1 class="about-title">Công ty TNHH TM DV Kỹ thuật Đại Phúc</h1>
          <p class="about-desc">
            Công ty TNHH TM DV Kỹ thuật Đại Phúc là doanh nghiệp chuyên cung cấp các giải pháp công nghệ thông tin, phát triển ứng dụng và dịch vụ gia công phần mềm. Công ty chủ yếu tập trung vào việc hỗ trợ chuyển đổi số và tối ưu hóa quy trình hoạt động cho các đối tác khách hàng trên thị trường.
          </p>
          <div class="about-brand-tagline">
            Đại Phúc: Công nghệ kết nối tự nhiên
          </div>
          <button class="btn-sage-pill" @click="showContactModal = true">
            Liên hệ hợp tác
          </button>
        </section>

        <!-- Right Side: Products (Sản phẩm) -->
        <section id="product" class="split-right-products">
          <div class="product-section-header">
            <h2 class="product-section-title">Sản phẩm của chúng tôi</h2>
          </div>

          <div class="product-cards-stack">
            <!-- Shield Card 1 -->
            <div class="organic-shield-card">
              <div class="card-icon-wrapper">
                <Monitor :size="28" :stroke-width="1.5" />
              </div>
              <div class="card-text-wrapper">
                <h3 class="card-title">{{ stationMonitorInfo.fileName }}</h3>
                <p class="card-desc">
                  Phần mềm quản lý và giám sát trạm quan trắc môi trường, cung cấp dữ liệu thời gian thực và phân tích chuyên sâu.
                </p>
              </div>
              <a 
                :href="stationMonitorInfo.downloadUrl" 
                :download="stationMonitorInfo.fileName"
                class="btn-terracotta-pill" 
                style="text-decoration: none; display: inline-flex; align-items: center; justify-content: center;"
                @click="triggerDownloadToast(stationMonitorInfo.fileName)"
              >
                Tải về
              </a>
            </div>

            <!-- Shield Card 2 (Master Station Setup) -->
            <div class="organic-shield-card-alt">
              <div class="card-icon-wrapper">
                <Server :size="28" :stroke-width="1.5" />
              </div>
              <div class="card-text-wrapper">
                <h3 class="card-title">{{ masterStationInfo.fileName }}</h3>
                <p class="card-desc">
                  Hệ thống quản lý trung tâm dữ liệu quan trắc, tích hợp dữ liệu và quản lý tập trung từ các trạm giám sát thành phần.
                </p>
              </div>
              <a 
                :href="masterStationInfo.downloadUrl" 
                :download="masterStationInfo.fileName"
                class="btn-terracotta-pill" 
                style="text-decoration: none; display: inline-flex; align-items: center; justify-content: center;"
                @click="triggerDownloadToast(masterStationInfo.fileName)"
              >
                Tải về
              </a>
            </div>
          </div>
        </section>
      </div>

      <!-- Minimalist Footer (Always at the bottom) -->
      <footer class="footer-bar-minimal">
        <div class="footer-copyright">
          © 2026 Đại Phúc Technology. Bảo lưu mọi quyền.
        </div>

        <div class="footer-social-links">
          <a href="#" class="footer-social-btn" aria-label="Facebook">
            <Facebook :size="16" />
          </a>
          <a href="#" class="footer-social-btn" aria-label="LinkedIn">
            <Linkedin :size="16" />
          </a>
          <a href="#" class="footer-social-btn" aria-label="YouTube">
            <Youtube :size="16" />
          </a>
        </div>
      </footer>

    </main>

    <!-- Success Toast Notification -->
    <transition name="fade">
      <div v-if="downloadSuccess" class="download-toast-sticky">
        <CheckCircle :size="18" />
        <span>Tải xuống {{ downloadedFile }} thành công!</span>
      </div>
    </transition>

    <!-- Contact Form Modal Overlay -->
    <transition name="fade">
      <div v-if="showContactModal" class="modal-backdrop" @click.self="showContactModal = false">
        <div class="modal-content-card">
          <button class="close-modal-trigger" @click="showContactModal = false" aria-label="Close modal">
            <X :size="20" />
          </button>
          
          <span class="modal-category">Liên hệ</span>
          <h3 class="modal-main-title">Gửi lời nhắn</h3>
          
          <form v-if="!submitSuccess" class="contact-form-content" @submit.prevent="submitContactForm">
            <div class="form-item">
              <label class="form-item-label" for="name">Họ và tên</label>
              <input 
                id="name" 
                type="text" 
                v-model="formName" 
                required 
                placeholder="Nguyễn Văn A" 
                class="form-item-input"
              />
            </div>
            
            <div class="form-item">
              <label class="form-item-label" for="email">Địa chỉ Email</label>
              <input 
                id="email" 
                type="email" 
                v-model="formEmail" 
                required 
                placeholder="name@company.com" 
                class="form-item-input"
              />
            </div>
            
            <div class="form-item">
              <label class="form-item-label" for="message">Lời nhắn của bạn</label>
              <textarea 
                id="message" 
                v-model="formMsg" 
                required 
                placeholder="Tôi muốn liên hệ về sản phẩm..." 
                class="form-item-input"
              ></textarea>
            </div>
            
            <button type="submit" class="btn-sage-pill btn-full" :disabled="isSubmitting">
              <span v-if="!isSubmitting">Gửi lời nhắn</span>
              <span v-else>Đang gửi...</span>
            </button>
          </form>
          
          <div v-else class="modal-success-state">
            <CheckCircle :size="48" class="success-check-icon" />
            <h4>Cảm ơn bạn!</h4>
            <p>Lời nhắn của bạn đã được gửi. Chúng tôi sẽ phản hồi sớm nhất có thể.</p>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<style scoped>
.site-root-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.product-cards-container {
  display: flex;
  justify-content: center;
  gap: 40px;
  flex-wrap: wrap;
  width: 100%;
}

/* Modal specific styles */
.modal-backdrop {
  position: fixed;
  inset: 0;
  background-color: rgba(45, 58, 49, 0.4);
  backdrop-filter: blur(4px);
  z-index: 10000;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 24px;
}

.modal-content-card {
  width: 100%;
  max-width: 480px;
  background-color: #FEFEFA;
  border-radius: 32px;
  border: 1px solid var(--border-color);
  padding: 40px;
  position: relative;
  box-shadow: var(--shadow-soft);
  animation: modal-zoom-in 0.3s cubic-bezier(0.16, 1, 0.3, 1);
}

.close-modal-trigger {
  position: absolute;
  top: 24px;
  right: 24px;
  background: none;
  border: none;
  color: var(--primary-color);
  cursor: pointer;
  transition: color var(--transition-fast);
}

.close-modal-trigger:hover {
  color: var(--text-color);
}

.modal-category {
  font-family: var(--font-sans);
  font-size: 12px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: var(--primary-color);
  margin-bottom: 8px;
  display: block;
}

.modal-main-title {
  font-family: var(--font-serif);
  font-size: 28px;
  color: var(--text-color);
  margin-bottom: 24px;
}

.contact-form-content {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.form-item {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.form-item-label {
  font-size: 13px;
  font-weight: 600;
  text-transform: uppercase;
  color: var(--primary-color);
}

.form-item-input {
  border: none;
  border-bottom: 1px solid var(--border-color);
  padding: 10px 0;
  background: transparent;
  font-family: var(--font-sans);
  font-size: 15px;
  color: var(--text-color);
  transition: border-color var(--transition-fast);
}

.form-item-input:focus {
  outline: none;
  border-bottom-color: var(--primary-color);
}

textarea.form-item-input {
  min-height: 80px;
  resize: vertical;
}

.btn-full {
  width: 100%;
}

.modal-success-state {
  text-align: center;
  padding: 30px 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 16px;
}

.success-check-icon {
  color: var(--primary-color);
}

.modal-success-state h4 {
  font-family: var(--font-serif);
  font-size: 24px;
  color: var(--text-color);
}

.modal-success-state p {
  color: #556257;
  font-size: 15px;
}

@keyframes modal-zoom-in {
  from {
    transform: scale(0.95);
    opacity: 0;
  }
  to {
    transform: scale(1);
    opacity: 1;
  }
}
</style>
