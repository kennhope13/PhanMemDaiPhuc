<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'
import { 
  Download, 
  Facebook, 
  Linkedin, 
  Youtube, 
  X, 
  Send,
  CheckCircle,
  Monitor,
  Server,
  Package,
  Cpu,
  ShieldCheck,
  ArrowRight,
  Sparkles,
  Layers,
  Network,
  Phone,
  Mail,
  MapPin,
  Clock,
  Calculator,
  FileText,
  Check,
  ChevronRight,
  ChevronLeft,
  HelpCircle,
  Award,
  Zap,
  Users,
  Building2,
  Settings,
  Shield,
  Search,
  Calendar,
  PhoneCall,
  Globe,
  Home,
  Laptop
} from 'lucide-vue-next'

useHead({
  title: 'Công ty TNHH TM DV Kỹ thuật Đại Phúc | Giải pháp CNTT & Thiết bị Mạng',
  meta: [
    { name: 'description', content: 'Công ty TNHH TM DV Kỹ thuật Đại Phúc chuyên cung cấp các giải pháp công nghệ thông tin, phát triển ứng dụng, thiết bị mạng chuyển mạch Switch, tường lửa Firewall, hệ thống Inverter và phần mềm giám sát quan trắc.' },
    { property: 'og:title', content: 'Công ty TNHH TM DV Kỹ thuật Đại Phúc' },
    { property: 'og:description', content: 'Giải pháp tối ưu, Khởi nguyên thịnh vượng - Đối tác tin cậy về hạ tầng mạng, an ninh thông tin và phần mềm kỹ thuật.' }
  ],
  link: [
    { rel: 'preconnect', href: 'https://fonts.googleapis.com' },
    { rel: 'preconnect', href: 'https://fonts.gstatic.com', crossorigin: '' },
    { rel: 'icon', type: 'image/png', href: '/logo.png' }
  ]
})

// Current navigation tab
const activeTab = ref('home')
const isMobileMenuOpen = ref(false)
const showContactModal = ref(false)
const downloadSuccess = ref(false)
const downloadedFile = ref('')
const isScrolled = ref(false)

// Full-width Banner Slideshow state
const currentSlide = ref(0)
const bannerSlides = ref([
  {
    id: 1,
    brandBig: 'ĐẠI PHÚC',
    brandSub: 'CÔNG TY KỸ THUẬT ĐẠI PHÚC',
    statements: [
      'Đại Phúc – Giải pháp hạ tầng mạng & CNTT tối ưu',
      'Đại Phúc – Thiết bị chính hãng, Khởi nguyên thịnh vượng'
    ],
    cardTag: 'GIẢI PHÁP HẠ TẦNG DOANH NGHIỆP',
    cardHeading: 'DẪN ĐẦU GIẢI PHÁP HẠ TẦNG CNTT & BẢO MẬT BỀN VỮNG',
    cardDesc: 'Chúng tôi cung cấp giải pháp trọn gói từ tư vấn, thiết kế, cung ứng thiết bị Switch Extreme, Juniper đến thi công và vận hành.',
    ctaText: 'XEM CHI TIẾT',
    targetTab: 'quotation',
    image: '/images/switch_extreme_24t_1785992540748.jpg'
  },
  {
    id: 2,
    brandBig: 'SCADA',
    brandSub: 'STATION MONITOR 3.0',
    statements: [
      'Đại Phúc – Phần mềm giám sát quan trắc thời gian thực',
      'Đại Phúc – Chuẩn hóa truyền dữ liệu trực tiếp về Sở TN&MT'
    ],
    cardTag: 'PHẦN MỀM & ĐO LƯỜNG TỰ ĐỘNG',
    cardHeading: 'HỆ THỐNG GIÁM SÁT & QUAN TRẮC MÔI TRƯỜNG THÔNG MINH',
    cardDesc: 'Bộ phần mềm Station Monitor & Master Station giám sát liên tục các thông số khí thải, nước thải và cảnh báo tự động.',
    ctaText: 'XEM CHI TIẾT',
    targetTab: 'solutions',
    image: '/dashboard.png'
  },
  {
    id: 3,
    brandBig: 'FIREWALL',
    brandSub: 'SONICWALL NS 5800',
    statements: [
      'Đại Phúc – Tường lửa thế hệ mới thông lượng 30Gbps',
      'Đại Phúc – Bảo vệ 8 triệu kết nối và chống Ransomware'
    ],
    cardTag: 'AN NINH THÔNG TIN TOÀN DIỆN',
    cardHeading: 'BẢO VỆ TOÀN DIỆN HỆ THỐNG MÁY CHỦ DATA CENTER',
    cardDesc: 'Ngăn chặn tấn công DDoS, mã độc tống tiền và kiểm soát lưu lượng mạng chuyên sâu theo thời gian thực.',
    ctaText: 'NHẬN BÁO GIÁ',
    targetTab: 'quotation',
    image: '/images/firewall_sonicwall_1785992634226.jpg'
  },
  {
    id: 4,
    brandBig: 'INVERTER',
    brandSub: 'INVERTER 3KVA RACK 19 INCH',
    statements: [
      'Đại Phúc – Sóng sin tinh khiết 110VDC/220VAC 2400W',
      'Đại Phúc – Cấp nguồn liên tục 24/7 cho tủ điều khiển'
    ],
    cardTag: 'NGUỒN ĐIỆN CÔNG NGHIỆP',
    cardHeading: 'GIẢI PHÁP NGUỒN ĐIỆN DỰ PHÒNG LIÊN TỤC 24/7',
    cardDesc: 'Bộ biến tần công nghiệp chuyên dụng cho các trạm biến áp, phòng máy chủ và nhà máy sản xuất.',
    ctaText: 'XEM BÁO GIÁ',
    targetTab: 'quotation',
    image: '/images/inverter_3kva_1785992656662.jpg'
  }
])

let slideTimer = null
const nextSlide = () => {
  currentSlide.value = (currentSlide.value + 1) % bannerSlides.value.length
}
const prevSlide = () => {
  currentSlide.value = (currentSlide.value - 1 + bannerSlides.value.length) % bannerSlides.value.length
}
const goToSlide = (idx) => {
  currentSlide.value = idx
}

const startSlideTimer = () => {
  stopSlideTimer()
  slideTimer = setInterval(() => {
    nextSlide()
  }, 4500)
}
const stopSlideTimer = () => {
  if (slideTimer) clearInterval(slideTimer)
}

// Release info
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

// Hardware products list
const hardwareProducts = ref([
  {
    id: 1,
    category: 'switch',
    categoryName: 'Switch Extreme',
    name: 'Switch Extreme L2 X435-24T-4S',
    priceNumber: 80000000,
    price: '80.000.000 VNĐ',
    description: 'Thiết bị chuyển mạch ExtremeSwitching™ X435-24T-4S:\n• 24 x 10/100/1000BASE-T access ports. Full / Half-Duplex (auto-sensing)\n• 4 x 1/2.5GBASE-X SFP uplink ports (unpopulated)\n• 1 x AC PSU tích hợp\n• 1 x 10/100/1000BASE-T out-of-band management port\n• 1 x USB A port cho external USB flash',
    icon: 'Package',
    image: '/images/switch_extreme_24t_1785992540748.jpg'
  },
  {
    id: 2,
    category: 'switch',
    categoryName: 'Switch Extreme',
    name: 'Switch Extreme L2 X435-24P-4S',
    priceNumber: 45000000,
    price: '45.000.000 VNĐ',
    description: 'Thiết bị chuyển mạch ExtremeSwitching™ X435-24P-4S:\n• 24 x 10/100/1000BASE-T 802.3at (30W) PoE ports. Full / Half-Duplex\n• 4 x 1/2.5GBASE-X SFP uplink ports (unpopulated)\n• 1 x AC PSU tích hợp cấp nguồn PoE mạnh mẽ\n• 1 x 10/100/1000BASE-T out-of-band management port\n• 1 x USB A port cho external USB flash',
    icon: 'Package',
    image: '/images/switch_extreme_24p_1785992572960.jpg'
  },
  {
    id: 3,
    category: 'firewall',
    categoryName: 'Firewall Bảo mật',
    name: 'Firewall SonicWall NS 5800',
    priceNumber: 212000000,
    price: '212.000.000 VNĐ',
    description: 'Tường lửa bảo mật cấp doanh nghiệp cao cấp:\n• 24x1GbE, 8x10G SFP+, 2 USB 3.0, 1 Console RJ-45, 1 Mgmt port\n• Firewall Inspection Throughput: 30 Gbps\n• IPS Throughput: 24 Gbps | VPN Throughput: 21 Gbps\n• Maximum Connections: 8.000.000 phiên kết nối đồng thời\n• Lưu trữ: 256 GB SSD (Nâng cấp tối đa 1 TB)',
    icon: 'ShieldCheck',
    image: '/images/firewall_sonicwall_1785992634226.jpg'
  },
  {
    id: 4,
    category: 'switch',
    categoryName: 'Switch Juniper',
    name: 'Switch Juniper EX4100-24P',
    priceNumber: 283000000,
    price: '283.000.000 VNĐ',
    description: 'Thiết bị chuyển mạch cao cấp Juniper Networks:\n• 24 cổng 10/100/1000BASE-T PoE+\n• 4 cổng 10GbE SFP+ Uplink tốc độ cao\n• 4 cổng 25GbE SFP28 Stacking / Uplink\n• Công suất PoE: 740 W / 1440 W\n• Năng lực chuyển mạch: 328 Gbps | Tốc độ chuyển tiếp: 244 Mpps\n• Nguồn JPSU-920-AC-AFO chuẩn công nghiệp',
    icon: 'Package',
    image: '/images/switch_juniper_1785992643984.jpg'
  },
  {
    id: 5,
    category: 'inverter',
    categoryName: 'Inverter Công nghiệp',
    name: 'INVERTER 3KVA-110VDC/220VAC',
    priceNumber: 35000000,
    price: '35.000.000 VNĐ',
    description: 'Bộ biến tần nguồn điện công nghiệp chuyên dụng:\n• Mã sản phẩm: IPS-DTA3000-1102-2U\n• Dạng sóng đầu ra: Pure Sine Wave (Sóng sin chuẩn tinh khiết)\n• Điện áp DC định mức: 110VDC (Dải làm việc: 90~145VDC)\n• Điện áp AC đầu ra: 220V 50Hz/60Hz ổn định\n• Công suất danh định: 3000VA / 2400W\n• Thiết kế tiêu chuẩn: 19 inch 2U Rack Type',
    icon: 'Cpu',
    image: '/images/inverter_3kva_1785992656662.jpg'
  },
  {
    id: 6,
    category: 'laptop',
    categoryName: 'Laptop Doanh Nghiệp',
    name: 'Laptop ASUS ExpertBook B1 (B1503CVA)',
    priceNumber: 33235000,
    price: '33.235.000 VNĐ',
    description: 'Laptop doanh nghiệp ASUS ExpertBook B1 (B1503CVA) chuẩn độ bền quân đội Mỹ:\n• Vi xử lý: Lên tới Intel® Core™ 7 / Core™ i7-1355U thế hệ mới\n• Bộ nhớ RAM: 2 khe SO-DIMM DDR5 5200MHz (hỗ trợ nâng cấp tối đa 64GB)\n• Ổ cứng: Hỗ trợ SSD kép NVMe PCIe® 4.0 lên tới 1TB (hỗ trợ RAID 0/1)\n• Màn hình: 15.6" FHD (1920x1080) Anti-Glare, bản lề mở rộng 180°\n• Cổng kết nối: 2x USB-C (PD & DP), 2x USB-A 3.2, 1x HDMI 1.4b, 1x RJ45 LAN\n• Bảo mật: Cảm biến vân tay, TPM 2.0, Nắp che Webcam, Khử tiếng ồn AI hai chiều',
    icon: 'Laptop',
    image: '/images/laptop_asus_expertbook_b1503.png'
  },
  {
    id: 7,
    category: 'pc',
    categoryName: 'Máy Tính Để Bàn',
    name: 'Máy tính để bàn ASUS ExpertCenter D700 SFF (D700SF)',
    priceNumber: 33350000,
    price: '33.350.000 VNĐ',
    description: 'Máy tính để bàn doanh nghiệp ASUS ExpertCenter D700 SFF (D700SF) siêu nhỏ gọn:\n• Vi xử lý: Lên tới Intel® Core™ Ultra 7-265 / Ultra 5-225, Chipset Intel® B860\n• Bộ nhớ RAM: 4 khe cắm DIMM DDR5 5600MHz (hỗ trợ mở rộng tối đa 128GB)\n• Đồ họa: Tùy chọn card đồ họa rời NVIDIA® RTX A400 4GB GDDR6\n• Lưu trữ: Tối đa 4 ổ (1x HDD 3.5" up to 2TB, 1x HDD 2.5", 2x M.2 PCIe® 4.0 SSD)\n• Thiết kế: Khung máy SFF nhỏ gọn không cần dụng cụ (Tool-less), đặt dọc/ngang linh hoạt\n• Cổng kết nối & Nguồn: 10 cổng USB (Type-C & Type-A), HDMI, DP, VGA, LAN Gigabit, Nguồn 80 PLUS Platinum\n• Độ bền & Bảo mật: Chuẩn quân đội Mỹ MIL-STD-810H, Chip TPM 2.0, Khử ồn AI hai chiều',
    icon: 'Monitor',
    image: '/images/pc_asus_expertcenter_d700sf.png'
  },
  {
    id: 8,
    category: 'monitor',
    categoryName: 'Màn Hình Máy Tính',
    name: 'Màn hình máy tính ASUS VY249HGR-R (23.8" / IPS / 120Hz / 1ms)',
    priceNumber: 2863500,
    price: '2.863.500 VNĐ',
    description: 'Màn hình máy tính ASUS VY249HGR-R 23.8 inch Full HD IPS 120Hz mượt mà:\n• Kích thước panel: 23.8 inch WLED/IPS, Tỷ lệ 16:9, Độ phân giải Full HD (1920 x 1080)\n• Tần số quét & Phản hồi: Tần số quét siêu mượt 120Hz, Thời gian phản hồi 1ms MPRT\n• Độ sáng & Tương phản: Độ sáng 300 cd/m², Tương phản tĩnh 1,500:1, Tương phản ASCR 100,000,000:1\n• Góc nhìn & Màu sắc: Góc nhìn siêu rộng 178°/178°, 16.7 triệu màu, Khử nhấp nháy Flicker-Free\n• Bảo vệ mắt Eye Care+: Lọc ánh sáng xanh TÜV Rheinland, Lớp phủ kháng khuẩn bề mặt\n• Tính năng nâng cao: Công nghệ VRR (Adaptive-Sync), GamePlus, QuickFit, Shadow Boost\n• Cổng kết nối: 1x HDMI (v1.4), 1x VGA (D-Sub), 1x Jack âm thanh 3.5mm, Chuẩn VESA (100x100mm)',
    icon: 'Monitor',
    image: '/images/monitor_asus_vy249hgr.png'
  },
  {
    id: 9,
    category: 'pc',
    categoryName: 'Máy Tính Để Bàn',
    name: 'Máy tính để bàn ASUS ExpertCenter P500 SFF (P500SV)',
    priceNumber: 25173500,
    price: '25.173.500 VNĐ',
    description: 'Máy tính để bàn doanh nghiệp ASUS ExpertCenter P500 SFF (P500SV) 8.6L nhỏ gọn & mạnh mẽ:\n• Vi xử lý: Lên tới Intel® Core™ i7-13620H (10 nhân, 16 luồng, 4.9GHz) / Intel® Core™ 7 240H (5.2GHz)\n• Bộ nhớ RAM: 2 khe cắm SO-DIMM DDR5 5600MHz kênh đôi (hỗ trợ tối đa 64GB)\n• Đồ họa: Đồ họa tích hợp Intel® UHD Graphics hoặc card rời NVIDIA® RTX A400 4GB GDDR6\n• Lưu trữ: Hỗ trợ SSD kép M.2 PCIe® 4.0 lên đến 2TB và 1 ổ cứng HDD 3.5" 2TB (Tổng tối đa 4TB)\n• Tản nhiệt & Khử ồn: Hệ thống tản nhiệt ống đồng tối ưu, khử tiếng ồn AI hai chiều ASUS\n• Khả năng mở rộng: Thân máy mở không cần dụng cụ (Tool-less), 1x PCIe 4.0 x16, 1x M.2 Wi-Fi6\n• Cổng kết nối: Mặt trước USB 3.2 Gen 1 (Type-C & Type-A), Jack combo; Mặt sau HDMI 1.4, DP 1.4, 4x USB 2.0, LAN 1GbE, Audio 7.1\n• Độ bền & Bảo mật: Chuẩn quân sự Hoa Kỳ MIL-STD-810H, Chip TPM 2.0, Nguồn 80 PLUS Platinum, ASUS ExpertGuardian',
    icon: 'Monitor',
    image: '/images/pc_asus_expertcenter_p500.png'
  },
  {
    id: 10,
    category: 'pc',
    categoryName: 'Máy Tính Để Bàn',
    name: 'Máy tính để bàn ASUS ExpertCenter D700 SFF (Ultra 5 / 16GB / 512GB / 3Y Onsite)',
    priceNumber: 37213770,
    price: '37.213.770 VNĐ',
    description: 'Máy tính để bàn ASUS ExpertCenter D700 SFF cấu hình AI-ready cao cấp (Part: 90PF05E1-M01600 / D700SF-0052250550):\n• Vi xử lý: Intel® Core™ Ultra 5 225 (10 nhân, 10 luồng, 20MB Cache, Turbo up to 4.9GHz, tích hợp NPU Intel® AI Boost)\n• Bộ nhớ RAM: 16GB DDR5 5600 MT/s U-DIMM (4 khe cắm DIMM, hỗ trợ nâng cấp tối đa 128GB)\n• Ổ cứng: 512GB M.2 2280 NVMe™ PCIe® 4.0 SSD (Hỗ trợ mở rộng SSD M.2 thứ 2 và khay HDD 2.5" & 3.5")\n• Chipset & Nguồn: Intel® B860 Chipset, Bộ nguồn 330W (80+ Platinum, Peak 660W)\n• Kết nối không dây & Mạng: Wi-Fi 6E (802.11ax Triple band) 2*2 + Bluetooth® 5.4, Intel Gigabit LAN\n• Cổng kết nối: 1x Type-C USB 3.2 Gen 2x2, 2x USB 3.2 Gen 2, 2x USB 3.2 Gen 1, 5x USB 2.0, HDMI 1.4, DP 1.4, VGA, Audio 7.1 kênh\n• Bảo mật & Độ bền: Chip TPM 2.0 phần cứng, khe khóa Kensington, ứng dụng AI ExpertMeet & MyASUS\n• Dịch vụ & Phụ kiện: Bảo hành 3 năm chính hãng tận nơi (3Y OnSite Service), kèm Bàn phím & Chuột có dây ASUS',
    icon: 'Monitor',
    image: '/images/pc_asus_d700sf_ultra5.png'
  },
  {
    id: 11,
    category: 'pc',
    categoryName: 'Máy Tính Để Bàn',
    name: 'Máy tính để bàn ASUS ExpertCenter D701 SFF (Core i5-14500 / 16GB / 512GB / 3Y Onsite)',
    priceNumber: 34454000,
    price: '34.454.000 VNĐ',
    description: 'Máy tính để bàn ASUS ExpertCenter D701 SFF hiệu năng vượt trội (Part: 90PF05N1-M031M0 / D701SER-5145002550):\n• Vi xử lý: Intel® Core™ i5-14500 (14 nhân, 20 luồng, 24MB Cache, xung nhịp tối đa lên đến 5.0GHz)\n• Đồ họa: Đồ họa tích hợp Intel® UHD Graphics 770\n• Bộ nhớ RAM: 16GB DDR5 5600MHz U-DIMM (4 khe cắm U-DIMM, hỗ trợ nâng cấp tối đa 128GB)\n• Ổ cứng: 512GB M.2 2280 NVMe™ PCIe® 4.0 SSD (Hỗ trợ mở rộng thêm M.2 2230 và khay HDD 2.5" & 3.5")\n• Chipset & Nguồn: Intel® B760 Chipset, Bộ nguồn 330W (80+ Platinum, công suất đỉnh 660W)\n• Kết nối mạng & Âm thanh: Intel WGI219V Gigabit LAN 10/100/1000 Mbps, Âm thanh 7.1 Channel High Definition\n• Cổng kết nối: Mặt trước 1x Type-C USB 3.2 Gen 2x2, 2x USB 3.2 Gen 2, 2x USB 2.0; Mặt sau HDMI 1.4, DP 1.4, VGA, 2x USB 3.2 Gen 1, 3x USB 2.0, LAN 1GbE, Audio 7.1\n• Bàn phím & Chuột: Tặng kèm Bàn phím kháng khuẩn công nghệ ASUS Antimicrobial Guard + Chuột quang có dây ASUS\n• Bảo mật & Dịch vụ: Chip bảo mật TPM 2.0, Khóa Kensington, Bảo hành 3 năm chính hãng tận nơi (3Y OnSite Service)',
    icon: 'Monitor',
    image: '/images/pc_asus_d701ser_i5.png'
  },
  {
    id: 12,
    category: 'pc',
    categoryName: 'Máy Tính Để Bàn',
    name: 'Máy tính để bàn ASUS ExpertCenter D701 SFF (Core i7-14700 / 16GB / 1TB SSD / 3Y Onsite)',
    priceNumber: 43447000,
    price: '43.447.000 VNĐ',
    description: 'Máy tính để bàn ASUS ExpertCenter D701 SFF đỉnh cao hiệu năng (Part: 90PF05N1-M04K70 / D701SER-7147002320):\n• Vi xử lý: Intel® Core™ i7-14700 (20 nhân, 28 luồng, 33MB Cache, xung nhịp Turbo lên tới 5.3GHz)\n• Đồ họa: Đồ họa tích hợp Intel® UHD Graphics 770\n• Bộ nhớ RAM: 16GB DDR5 5600 MT/s U-DIMM (4 khe cắm U-DIMM, nâng cấp tối đa 128GB)\n• Ổ cứng: 1TB M.2 2280 NVMe™ PCIe® 4.0 SSD dung lượng khủng (Hỗ trợ thêm M.2 2230 và khay HDD 2.5" & 3.5")\n• Chipset & Nguồn: Intel® B760 Chipset, Bộ nguồn 330W (80+ Platinum, công suất đỉnh 660W)\n• Kết nối không dây & Mạng: Wi-Fi 6E (802.11ax Triple band) 2*2 + Bluetooth® 5.4, Intel Gigabit LAN\n• Cổng kết nối phong phú: 1x FLEX I/O (HDMI 2.1), 1x HDMI 1.4, 1x DP 1.4, 1x VGA, 1x Type-C USB 3.2 Gen 2x2 (20 Gbps), 2x USB 3.2 Gen 2 (10 Gbps), 2x USB 3.2 Gen 1, 5x USB 2.0, Đầu đọc thẻ SD/MMC 2-in-1, Audio 7.1\n• Bàn phím & Chuột: Trọn bộ Bàn phím & Chuột ASUS AW311WD Black chính hãng\n• Bảo mật & Dịch vụ: Chip TPM 2.0 phần cứng, Khóa Kensington, Bảo hành chính hãng 3 năm tận nơi (3Y OnSite Service)',
    icon: 'Monitor',
    image: '/images/pc_asus_d701ser_i7.png'
  }
])

// Quotation filter & selection
const selectedQuotationCategory = ref('all')
const filteredProducts = computed(() => {
  if (selectedQuotationCategory.value === 'all') return hardwareProducts.value
  return hardwareProducts.value.filter(p => p.category === selectedQuotationCategory.value)
})

// Quotation Estimator tool state
const estimatorItems = ref([
  { id: 1, name: 'Switch Extreme L2 X435-24T-4S', price: 80000000, qty: 1, selected: true },
  { id: 2, name: 'Switch Extreme L2 X435-24P-4S', price: 45000000, qty: 0, selected: false },
  { id: 3, name: 'Firewall SonicWall NS 5800', price: 212000000, qty: 0, selected: false },
  { id: 4, name: 'Switch Juniper EX4100-24P', price: 283000000, qty: 0, selected: false },
  { id: 5, name: 'INVERTER 3KVA-110VDC/220VAC', price: 35000000, qty: 1, selected: true },
  { id: 6, name: 'Bản quyền Phần mềm Station Monitor 3.0', price: 25000000, qty: 1, selected: true },
  { id: 7, name: 'Bản quyền Phần mềm Master Station 3.0', price: 50000000, qty: 0, selected: false },
  { id: 8, name: 'Laptop ASUS ExpertBook B1 (B1503CVA)', price: 33235000, qty: 0, selected: false },
  { id: 9, name: 'PC ASUS ExpertCenter D700 SFF (D700SF)', price: 33350000, qty: 0, selected: false },
  { id: 10, name: 'PC ASUS ExpertCenter P500 SFF (P500SV)', price: 25173500, qty: 0, selected: false },
  { id: 11, name: 'PC ASUS ExpertCenter D700 SFF Ultra 5 (16GB/512GB)', price: 37213770, qty: 0, selected: false },
  { id: 12, name: 'PC ASUS ExpertCenter D701 SFF i5-14500 (16GB/512GB)', price: 34454000, qty: 0, selected: false },
  { id: 13, name: 'PC ASUS ExpertCenter D701 SFF i7-14700 (16GB/1TB)', price: 43447000, qty: 0, selected: false },
  { id: 14, name: 'Màn hình ASUS VY249HGR-R 23.8" 120Hz', price: 2863500, qty: 0, selected: false }
])

const includeInstallation = ref(true)
const includeExtendedWarranty = ref(false)

const estimatedTotal = computed(() => {
  let sum = 0
  estimatorItems.value.forEach(item => {
    if (item.selected && item.qty > 0) {
      sum += item.price * item.qty
    }
  })
  if (includeInstallation.value && sum > 0) {
    sum += Math.min(sum * 0.05, 50000000) // 5% service fee max 50m
  }
  if (includeExtendedWarranty.value && sum > 0) {
    sum += sum * 0.08 // 8% extended warranty
  }
  return sum
})

const formatCurrency = (val) => {
  return new Intl.NumberFormat('vi-VN', { style: 'currency', currency: 'VND' }).format(val)
}

const decreaseQty = (item) => {
  if (item.qty > 0) {
    item.qty--
    if (item.qty === 0) item.selected = false
  }
}

const increaseQty = (item) => {
  item.qty++
  item.selected = true
}

// Projects list
const projectsList = ref([
  {
    id: 1,
    title: 'Triển khai Biến tần Inverter 3KVA-110VDC tại Nhà máy Điện',
    client: 'Nhà máy Nhiệt điện Duyên Hải',
    year: '2025',
    category: 'Inverter & Nguồn Công nghiệp',
    desc: 'Cung cấp và lắp đặt bộ biến tần INVERTER 3KVA 110VDC/220VAC sóng sin chuẩn tinh khiết, cấp nguồn liên tục cho hệ thống bảo vệ và điều khiển SCADA.',
    image: '/images/inverter_3kva_1785992656662.jpg'
  },
  {
    id: 2,
    title: 'Nâng cấp Hạ tầng Mạng Chuyển Mạch Switch Extreme X435-24T',
    client: 'Khu Công nghệ Cao TP.HCM',
    year: '2025',
    category: 'Hạ tầng Mạng Switch',
    desc: 'Trang bị cụm Switch Extreme L2 X435-24T-4S 24 cổng Gigabit, 4 cổng quang SFP, đáp ứng truyền tải dữ liệu tốc độ cao cho toàn bộ khu phức hợp.',
    image: '/images/switch_extreme_24t_1785992540748.jpg'
  },
  {
    id: 3,
    title: 'Hệ thống Cấp nguồn PoE+ Switch Extreme X435-24P',
    client: 'Chuỗi Tòa nhà Văn phòng & TTTM',
    year: '2025',
    category: 'Hạ tầng Mạng PoE+',
    desc: 'Triển khai dòng Switch Extreme L2 X435-24P-4S cấp nguồn chuẩn 802.3at 30W cho hơn 100 Camera an ninh IP và hệ thống phát Wifi Access Point.',
    image: '/images/switch_extreme_24p_1785992572960.jpg'
  },
  {
    id: 4,
    title: 'Triển khai Tường lửa SonicWall NS 5800 Bảo vệ Cụm Máy chủ',
    client: 'Tổng công ty Dịch vụ Tài chính & Chứng khoán',
    year: '2024',
    category: 'An ninh mạng & Bảo mật',
    desc: 'Thiết lập hệ thống tường lửa SonicWall NS 5800 kiểm soát lưu lượng 30Gbps, bảo vệ cụm máy chủ và ngăn chặn tấn công mạng, xâm nhập trái phép 24/7.',
    image: '/images/firewall_sonicwall_1785992634226.jpg'
  },
  {
    id: 5,
    title: 'Hạ tầng Mạng Cốt lõi Juniper EX4100-24P Đa Điểm',
    client: 'Tập đoàn Viễn thông & Trung tâm Dữ liệu',
    year: '2024',
    category: 'Switch Juniper',
    desc: 'Cấu hình hệ thống Juniper EX4100-24P hỗ trợ Stacking 25GbE, năng lực chuyển mạch 328 Gbps, đảm bảo kết nối ổn định không gián đoạn (High Availability).',
    image: '/images/switch_juniper_1785992643984.jpg'
  },
  {
    id: 6,
    title: 'Hệ thống Giám sát & Quan trắc Khí thải Môi trường Tự động',
    client: 'Tập đoàn Công nghiệp & Khu Chế xuất Miền Nam',
    year: '2025',
    category: 'Phần mềm & SCADA',
    desc: 'Ứng dụng phần mềm Station Monitor thu thập dữ liệu quan trắc khí thải, nước thải từ các trạm phân tán và tự động truyền về Sở TN&MT theo quy chuẩn.',
    image: '/images/switch_extreme_24p_1785992572960.jpg'
  }
])

// Blog posts
const blogPosts = ref([
  {
    id: 1,
    title: 'Đánh giá chi tiết dòng Switch Extreme Networks X435 cho doanh nghiệp',
    date: '15/03/2026',
    category: 'Thiết bị mạng',
    summary: 'Phân tích các ưu điểm vượt trội về hiệu năng chuyển mạch L2, tính năng cấp nguồn PoE+ 30W và khả năng mở rộng với cổng quang 2.5G/SFP.'
  },
  {
    id: 2,
    title: 'Giải pháp phòng thủ mạng toàn diện với Firewall SonicWall thế hệ mới',
    date: '28/02/2026',
    category: 'Bảo mật thông tin',
    summary: 'Tại sao SonicWall NS 5800 là lựa chọn tối ưu cho các trung tâm dữ liệu lớn với khả năng xử lý đồng thời 8 triệu kết nối và chống mã độc thời gian thực.'
  },
  {
    id: 3,
    title: 'Tiêu chuẩn truyền nhận dữ liệu trạm quan trắc tự động theo Thông tư mới',
    date: '10/01/2026',
    category: 'Giải pháp phần mềm',
    summary: 'Hướng dẫn chuẩn hóa giao thức kết nối từ trạm đo hiện trường về trung tâm giám sát Master Station đáp ứng 100% yêu cầu kỹ thuật của cơ quan quản lý.'
  }
])

// Contact form state
const formName = ref('')
const formEmail = ref('')
const formPhone = ref('')
const formService = ref('Tư vấn báo giá thiết bị')
const formMsg = ref('')
const isSubmitting = ref(false)
const submitSuccess = ref(false)

const switchTab = (tabId) => {
  activeTab.value = tabId
  isMobileMenuOpen.value = false
  window.scrollTo({ top: 0, behavior: 'smooth' })
}

const openQuoteFormWithProduct = (productName, price) => {
  formService.value = `Yêu cầu báo giá: ${productName}`
  formMsg.value = `Tôi quan tâm đến sản phẩm ${productName} (Đơn giá tham khảo: ${price}). Vui lòng gửi báo giá chi tiết và chính sách chiết khấu.`
  showContactModal.value = true
}

const submitQuotationEstimate = () => {
  const selectedList = estimatorItems.value
    .filter(i => i.selected && i.qty > 0)
    .map(i => `${i.name} (SL: ${i.qty})`)
    .join(', ')
  
  formService.value = 'Dự toán báo giá trực tuyến'
  formMsg.value = `Yêu cầu báo giá dự toán:\n- Thiết bị: ${selectedList}\n- Dịch vụ lắp đặt: ${includeInstallation.value ? 'Có' : 'Không'}\n- Bảo hành mở rộng: ${includeExtendedWarranty.value ? 'Có' : 'Không'}\n- Tổng dự toán ước tính: ${formatCurrency(estimatedTotal.value)}`
  showContactModal.value = true
}

const submitContactForm = () => {
  isSubmitting.value = true
  setTimeout(() => {
    isSubmitting.value = false
    submitSuccess.value = true
    formName.value = ''
    formEmail.value = ''
    formPhone.value = ''
    formMsg.value = ''
    setTimeout(() => {
      submitSuccess.value = false
      showContactModal.value = false
    }, 2500)
  }, 1000)
}

const triggerDownloadToast = (filename) => {
  downloadedFile.value = filename
  downloadSuccess.value = true
  setTimeout(() => {
    downloadSuccess.value = false
  }, 5000)
}

const formatDescription = (desc) => {
  if (!desc) return '';
  const lines = desc.split('\n');
  let html = '';
  let inList = false;
  
  lines.forEach(line => {
    line = line.trim();
    if (line.startsWith('•') || line.startsWith('-')) {
      if (!inList) {
        html += '<ul class="hw-feature-list">';
        inList = true;
      }
      const content = line.substring(1).trim();
      html += `<li>${content}</li>`;
    } else {
      if (inList) {
        html += '</ul>';
        inList = false;
      }
      if (line) {
        html += `<p class="hw-desc-paragraph">${line}</p>`;
      }
    }
  });
  
  if (inList) {
    html += '</ul>';
  }
  
  return html;
}

const handleScroll = () => {
  if (typeof window !== 'undefined') {
    isScrolled.value = window.scrollY > 60
  }
}

onMounted(async () => {
  startSlideTimer()
  if (typeof window !== 'undefined') {
    window.addEventListener('scroll', handleScroll, { passive: true })
    handleScroll()
  }

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
    console.error('Error Power-Monitor:', err)
  }

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
    console.error('Error Master-Station:', err)
  }

  if (typeof window !== 'undefined') {
    window.addEventListener('keydown', handleGlobalKeydown)
  }
})

// Quick Search Modal State & Handlers
const showSearchModal = ref(false)
const searchQuery = ref('')
const searchInputRef = ref(null)

const openSearch = () => {
  showSearchModal.value = true
  searchQuery.value = ''
  nextTick(() => {
    searchInputRef.value?.focus()
  })
}

const handleGlobalKeydown = (e) => {
  if ((e.ctrlKey || e.metaKey) && e.key === 'k') {
    e.preventDefault()
    openSearch()
  } else if (e.key === 'Escape' && showSearchModal.value) {
    showSearchModal.value = false
  }
}

const searchResults = computed(() => {
  const q = searchQuery.value.trim().toLowerCase()
  if (!q) return { products: [], projects: [], blogs: [] }

  const products = hardwareProducts.value.filter(p => 
    p.name.toLowerCase().includes(q) || 
    p.categoryName.toLowerCase().includes(q) ||
    p.description.toLowerCase().includes(q)
  )

  const projects = projectsList.value.filter(p => 
    p.title.toLowerCase().includes(q) || 
    p.client.toLowerCase().includes(q) || 
    p.category.toLowerCase().includes(q) ||
    p.desc.toLowerCase().includes(q)
  )

  const blogs = blogPosts.value.filter(b => 
    b.title.toLowerCase().includes(q) || 
    b.category.toLowerCase().includes(q) || 
    b.summary.toLowerCase().includes(q)
  )

  return { products, projects, blogs }
})

const totalSearchResults = computed(() => {
  const { products, projects, blogs } = searchResults.value
  return products.length + projects.length + blogs.length
})

const handleSearchResultClick = (type, item) => {
  showSearchModal.value = false
  if (type === 'product') {
    switchTab('quotation')
    openQuoteFormWithProduct(item.name, item.price)
  } else if (type === 'project') {
    switchTab('projects')
  } else if (type === 'blog') {
    switchTab('blog')
  }
}

onUnmounted(() => {
  stopSlideTimer()
  if (typeof window !== 'undefined') {
    window.removeEventListener('scroll', handleScroll)
    window.removeEventListener('keydown', handleGlobalKeydown)
  }
})
</script>

<template>
  <div class="site-root-container">
    <!-- Ambient Tech Grid -->
    <div class="tech-bg-grid" />
    <div class="tech-glow-top" />


    <!-- Main Sticky Header (ATSolar style: Center logo with navigation tabs floating over the hero banner) -->
    <header 
      class="top-header-wrapper"
      :class="{
        'header-on-banner': activeTab === 'home' && !isScrolled,
        'header-scrolled': isScrolled || activeTab !== 'home'
      }"
    >
      <div class="top-logo-bar">
        
        <!-- Left Navigation Group (ATSolar Style) -->
        <nav class="top-nav-menu nav-group-left">
          <button 
            class="top-nav-link nav-home-icon-btn" 
            :class="{ active: activeTab === 'home' }"
            @click="switchTab('home')"
            aria-label="Trang chủ"
          >
            <span class="home-icon-circle">
              <Home :size="16" />
            </span>
          </button>
          <button 
            class="top-nav-link" 
            :class="{ active: activeTab === 'about' }"
            @click="switchTab('about')"
          >
            GIỚI THIỆU
          </button>
          <button 
            class="top-nav-link" 
            :class="{ active: activeTab === 'solutions' }"
            @click="switchTab('solutions')"
          >
            GIẢI PHÁP
          </button>
          <button 
            class="top-nav-link" 
            :class="{ active: activeTab === 'quotation' }"
            @click="switchTab('quotation')"
          >
            BÁO GIÁ
          </button>
        </nav>

        <!-- Center Brand Logo -->
        <a href="#" class="brand-logo-link center-brand" @click.prevent="switchTab('home')">
          <img src="/logo.png?v=2" alt="Đại Phúc Logo" class="logo-image-top" />
          <div class="logo-text-group">
            <span class="logo-brand-name">ĐẠI PHÚC</span>
          </div>
        </a>

        <!-- Right Navigation Group (ATSolar Style) -->
        <nav class="top-nav-menu nav-group-right">
          <button 
            class="top-nav-link" 
            :class="{ active: activeTab === 'projects' }"
            @click="switchTab('projects')"
          >
            DỰ ÁN
          </button>
          <button 
            class="top-nav-link" 
            :class="{ active: activeTab === 'blog' }"
            @click="switchTab('blog')"
          >
            BLOG
          </button>
          <button 
            class="top-nav-link" 
            :class="{ active: activeTab === 'contact' }"
            @click="switchTab('contact')"
          >
            LIÊN HỆ
          </button>
          
          <button class="top-nav-link nav-cta-btn" @click="switchTab('quotation')">
            <Calculator :size="14" />
            <span>DỰ TOÁN</span>
          </button>

          <!-- Search Trigger Button (ATSolar Style) -->
          <button class="top-nav-link nav-search-btn" @click="openSearch" aria-label="Tìm kiếm nhanh">
            <Search :size="16" />
          </button>
        </nav>

        <!-- Mobile Menu Toggle Button -->
        <button class="mobile-nav-toggle" @click="isMobileMenuOpen = !isMobileMenuOpen" aria-label="Toggle Menu">
          <X v-if="isMobileMenuOpen" :size="24" />
          <span v-else style="font-size: 20px; font-weight: bold;">☰</span>
        </button>
      </div>

      <!-- Mobile Dropdown Menu -->
      <div v-if="isMobileMenuOpen" class="mobile-nav-dropdown">
        <button class="top-nav-link" :class="{ active: activeTab === 'home' }" @click="switchTab('home')">Trang chủ</button>
        <button class="top-nav-link" :class="{ active: activeTab === 'about' }" @click="switchTab('about')">Giới thiệu</button>
        <button class="top-nav-link" :class="{ active: activeTab === 'solutions' }" @click="switchTab('solutions')">Giải pháp</button>
        <button class="top-nav-link" :class="{ active: activeTab === 'quotation' }" @click="switchTab('quotation')">Báo giá & Dự toán</button>
        <button class="top-nav-link" :class="{ active: activeTab === 'projects' }" @click="switchTab('projects')">Dự án</button>
        <button class="top-nav-link" :class="{ active: activeTab === 'blog' }" @click="switchTab('blog')">Tin tức</button>
        <button class="top-nav-link" :class="{ active: activeTab === 'contact' }" @click="switchTab('contact')">Liên hệ</button>
        <button class="top-nav-link" style="color: var(--primary-color); font-weight: 700;" @click="openSearch; isMobileMenuOpen = false">🔍 Tìm kiếm</button>
      </div>
    </header>

    <!-- ================= FULL-SCREEN WIDTH BANNER SLIDESHOW (ATS STYLE) ================= -->
    <section 
      v-if="activeTab === 'home'" 
      class="fullwidth-hero-banner"
      @mouseenter="stopSlideTimer"
      @mouseleave="startSlideTimer"
    >
      <div class="fullwidth-banner-backdrop" />
      <div class="banner-dynamic-orb banner-orb-1" />
      <div class="banner-dynamic-orb banner-orb-2" />

      <!-- Slide Nav Arrows (Edge to edge) -->
      <button class="fullwidth-banner-nav prev" @click="prevSlide" aria-label="Previous Slide">
        <ChevronLeft :size="28" />
      </button>
      <button class="fullwidth-banner-nav next" @click="nextSlide" aria-label="Next Slide">
        <ChevronRight :size="28" />
      </button>

      <!-- Main Slide Grid with Dynamic Key Transition -->
      <transition name="banner-slide" mode="out-in">
        <div class="fullwidth-slide-inner" :key="currentSlide">
          
          <!-- Left Side: Big Brand & Green Statement Slogans -->
          <div class="hero-brand-col">
            <div class="hero-main-logo-text animated-slide-down">{{ bannerSlides[currentSlide].brandBig }}</div>
            <div class="hero-sub-brand animated-slide-sub">{{ bannerSlides[currentSlide].brandSub }}</div>
            
            <div class="hero-bullet-statements">
              <div 
                v-for="(stmt, sIdx) in bannerSlides[currentSlide].statements" 
                :key="sIdx" 
                class="hero-statement-item"
                :style="{ animationDelay: `${(sIdx + 1) * 0.15}s` }"
              >
                <span class="hero-statement-dot"></span>
                <span>{{ stmt }}</span>
              </div>
            </div>

            <!-- Slide Indicator Dots -->
            <div class="hero-dots-container">
              <button 
                v-for="(slide, idx) in bannerSlides" 
                :key="slide.id" 
                class="hero-dot" 
                :class="{ active: currentSlide === idx }"
                @click="goToSlide(idx)"
                :aria-label="`Slide ${idx + 1}`"
              />
            </div>
          </div>

          <!-- Right Side: Highlighted Feature Card with CTA -->
          <div class="hero-feature-col">
            <div class="hero-feature-card floating-motion">
              <span class="hero-card-tag">{{ bannerSlides[currentSlide].cardTag }}</span>
              <h2 class="hero-card-heading">{{ bannerSlides[currentSlide].cardHeading }}</h2>
              <p class="hero-card-desc">{{ bannerSlides[currentSlide].cardDesc }}</p>
              
              <button class="hero-card-cta-btn" @click="switchTab(bannerSlides[currentSlide].targetTab)">
                {{ bannerSlides[currentSlide].ctaText }}
              </button>

              <!-- Device Image Preview Box -->
              <div class="hero-device-preview">
                <img :src="bannerSlides[currentSlide].image" :alt="bannerSlides[currentSlide].cardHeading" class="pulse-device-img" />
              </div>
            </div>
          </div>

        </div>
      </transition>

      <!-- Bottom Green Ribbon with Hotline only (removed domain) -->
      <div class="hero-bottom-ribbon">
        <div class="ribbon-pill-container ribbon-pulsing">
          <a href="tel:0355331720" class="ribbon-item-phone">
            <Phone :size="20" class="phone-ring-icon" />
            <span>Tư vấn Hotline: 0355 331 720</span>
          </a>
        </div>
      </div>
    </section>

    <!-- Main Content Body -->
    <main class="main-content-scrollable">
      <div class="container">
        
        <!-- ================= TAB 1: TRANG CHỦ (HOME) ================= -->
        <section v-show="activeTab === 'home'" class="view-section" style="padding-top: 36px;">
          
          <!-- Featured Products Grid with Price -->
          <div class="section-spacer" style="margin-top: 0;">
            <div class="section-header-center">
              <div class="badge-tag">Sản phẩm nổi bật</div>
              <h2 class="section-title">Thiết Bị & Phần Cứng Bán Chạy</h2>
              <p class="section-subtitle">Cam kết chính hãng, đầy đủ CO/CQ, chính sách giá dự án cạnh tranh nhất</p>
            </div>

            <div class="hardware-grid">
              <div v-for="product in hardwareProducts" :key="product.id" class="hardware-card">
                <div v-if="product.image" class="hw-card-image-wrapper">
                  <img :src="product.image" :alt="product.name" class="hw-card-image" loading="lazy" />
                </div>
                
                <div class="hw-card-meta">
                  <span class="hw-category-tag">{{ product.categoryName }}</span>
                  <span class="hw-price-tag">{{ product.price }}</span>
                </div>

                <h3 class="hw-card-title">{{ product.name }}</h3>
                
                <div class="hw-card-desc" v-html="formatDescription(product.description)"></div>
                
                <div class="hw-card-actions">
                  <button 
                    class="btn-primary btn-full" 
                    @click="openQuoteFormWithProduct(product.name, product.price)"
                  >
                    Nhận báo giá
                  </button>
                </div>
              </div>
            </div>

            <div style="text-align: center; margin-top: 32px;">
              <button class="btn-outline" @click="switchTab('quotation')">
                <span>Xem toàn bộ bảng giá và công cụ tính dự toán</span>
                <ChevronRight :size="16" />
              </button>
            </div>
          </div>
        </section>

        <!-- ================= TAB 2: GIỚI THIỆU (ABOUT) ================= -->
        <section v-show="activeTab === 'about'" class="view-section">
          <!-- ATSolar Style Subpage Header Banner -->
          <div class="subpage-hero-banner">
            <div class="subpage-banner-container">
              <div class="subpage-breadcrumb">
                <a @click="switchTab('home')">Trang chủ</a>
                <span class="crumb-sep">/</span>
                <span class="crumb-current">Giới thiệu</span>
              </div>
              <h1 class="subpage-main-heading">CHÚNG TÔI LÀ AI ?</h1>
            </div>
          </div>

          <div class="about-hero-card">
            <h2 class="about-title">Công ty TNHH TM DV Kỹ thuật Đại Phúc</h2>
            <p class="about-desc">
              Được thành lập với sứ mệnh đồng hành cùng các doanh nghiệp trong kỷ nguyên số hóa, Đại Phúc cung cấp chuỗi giải pháp khép kín từ tư vấn thiết kế hạ tầng CNTT, cung ứng thiết bị mạng, bảo mật tường lửa đến phát triển phần mềm chuyên dụng và dịch vụ bảo trì kỹ thuật 24/7.
            </p>

            <div class="about-highlights-grid">
              <div class="highlight-box">
                <span class="highlight-label">Sứ mệnh</span>
                <span class="highlight-value">Giải pháp tối ưu, Khởi nguyên thịnh vượng</span>
              </div>
              <div class="highlight-box">
                <span class="highlight-label">Thế mạnh cốt lõi</span>
                <span class="highlight-value">Hạ tầng mạng, An ninh & Tự động hóa</span>
              </div>
              <div class="highlight-box">
                <span class="highlight-label">Tiêu chuẩn dịch vụ</span>
                <span class="highlight-value">Chính hãng 100% - Hỗ trợ kỹ thuật 24/7</span>
              </div>
            </div>

            <div class="workflow-section">
              <h2 class="workflow-title">Quy Trình Triển Khai Dịch Vụ Chuẩn 4 Bước</h2>
              <div class="workflow-steps-grid">
                <div class="step-card">
                  <div class="step-badge">Bước 1</div>
                  <h4>Khảo Sát & Tư Vấn</h4>
                  <p>Lắng nghe nhu cầu, khảo sát thực địa hiện trường và đánh giá hạ tầng mạng/phần mềm hiện hữu.</p>
                </div>
                <div class="step-card">
                  <div class="step-badge">Bước 2</div>
                  <h4>Lập Dự Toán & Báo Giá</h4>
                  <p>Thiết kế sơ đồ giải pháp chi tiết, lên cấu hình thiết bị phù hợp và lập bảng dự toán chi phí tối ưu.</p>
                </div>
                <div class="step-card">
                  <div class="step-badge">Bước 3</div>
                  <h4>Thi Công & Cấu Hình</h4>
                  <p>Cung ứng thiết bị chính hãng, lắp đặt tiêu chuẩn, cấu hình tối ưu hiệu năng và kiểm thử tải thực tế.</p>
                </div>
                <div class="step-card">
                  <div class="step-badge">Bước 4</div>
                  <h4>Bàn Giao & Bảo Trì 24/7</h4>
                  <p>Chuyển giao công nghệ, hướng dẫn vận hành chi tiết và cung cấp gói bảo hành bảo trì liên tục.</p>
                </div>
              </div>
            </div>

            <div style="margin-top: 36px;">
              <button class="btn-primary" @click="showContactModal = true">
                <span>Liên hệ hợp tác cùng Đại Phúc</span>
                <ArrowRight :size="16" />
              </button>
            </div>
          </div>
        </section>

        <!-- ================= TAB 3: GIẢI PHÁP (SOLUTIONS) ================= -->
        <section v-show="activeTab === 'solutions'" class="view-section">
          <!-- ATSolar Style Subpage Header Banner -->
          <div class="subpage-hero-banner">
            <div class="subpage-banner-container">
              <div class="subpage-breadcrumb">
                <a @click="switchTab('home')">Trang chủ</a>
                <span class="crumb-sep">/</span>
                <span class="crumb-current">Giải pháp</span>
              </div>
              <h1 class="subpage-main-heading">GIẢI PHÁP KỸ THUẬT</h1>
            </div>
          </div>

          <div class="solutions-list-stack">
            <!-- Solution 1 -->
            <div class="solution-detail-card">
              <div class="solution-detail-content">
                <div class="solution-badge-category">Giải pháp phần mềm & SCADA</div>
                <h2>Hệ Thống Giám Sát & Quản Trị Trạm Quan Trắc Tự Động</h2>
                <p>
                  Bộ giải pháp phần mềm <strong>Station Monitor</strong> và <strong>Master Station</strong> do Đại Phúc phát triển cung cấp khả năng thu thập, xử lý và giám sát liên tục các thông số môi trường (khí thải, nước thải, nhiệt độ, độ ẩm, áp suất) thời gian thực.
                </p>
                <ul class="solution-bullet-points">
                  <li><Check :size="16" /> Tự động kết nối, cảnh báo ngưỡng thông minh qua SMS/Email/Telegram.</li>
                  <li><Check :size="16" /> Chuẩn hóa giao thức truyền nhận dữ liệu trực tiếp về cơ quan quản lý nhà nước.</li>
                  <li><Check :size="16" /> Báo cáo thống kê, biểu đồ xu hướng và phân tích dữ liệu lịch sử trực quan.</li>
                </ul>
                <div class="solution-cta-row">
                  <button class="btn-outline" @click="openQuoteFormWithProduct('Phần mềm Station Monitor', '25.000.000 VNĐ')">
                    Yêu cầu demo
                  </button>
                </div>
              </div>
            </div>

            <!-- Solution 2 -->
            <div class="solution-detail-card">
              <div class="solution-detail-content">
                <div class="solution-badge-category">Hạ tầng mạng doanh nghiệp</div>
                <h2>Giải Pháp Chuyển Mạch Switch Extreme & Juniper Đỉnh Cao</h2>
                <p>
                  Hạ tầng mạng chuyển mạch tốc độ Gigabit & 10G/25G với độ trễ siêu thấp, hỗ trợ PoE+ 30W cấp nguồn trực tiếp cho Camera, thiết bị IoT và trạm quan trắc.
                </p>
                <ul class="solution-bullet-points">
                  <li><Check :size="16" /> Dòng ExtremeSwitching™ X435: 24 cổng Gigabit, 4 cổng quang SFP, quản lý Out-of-band.</li>
                  <li><Check :size="16" /> Dòng Juniper EX4100-24P: Năng lực chuyển mạch 328 Gbps, Stacking 25G tốc độ cực cao.</li>
                  <li><Check :size="16" /> Khả năng chịu lỗi và tính sẵn sàng cao (High Availability).</li>
                </ul>
                <div class="solution-cta-row">
                  <button class="btn-primary" @click="switchTab('quotation')">
                    <span>Xem bảng giá Switch</span>
                    <ArrowRight :size="16" />
                  </button>
                </div>
              </div>
            </div>

            <!-- Solution 3 -->
            <div class="solution-detail-card">
              <div class="solution-detail-content">
                <div class="solution-badge-category">An toàn thông tin & Bảo mật</div>
                <h2>Giải Pháp Tường Lửa Thế Hệ Mới SonicWall NS 5800</h2>
                <p>
                  Bảo vệ toàn diện hệ thống máy chủ và mạng nội bộ trước các cuộc tấn công DDoS, mã độc Ransomware, lỗ hổng Zero-day với hiệu năng xử lý 30 Gbps và 8 triệu phiên kết nối đồng thời.
                </p>
                <ul class="solution-bullet-points">
                  <li><Check :size="16" /> Kiểm tra gói tin sâu (Deep Packet Inspection) thời gian thực không làm giảm tốc độ mạng.</li>
                  <li><Check :size="16" /> Hệ thống IPS Throughput 24 Gbps, VPN mã hóa bảo mật 21 Gbps.</li>
                  <li><Check :size="16" /> Hỗ trợ cấu hình High Availability dự phòng nóng 1+1.</li>
                </ul>
                <div class="solution-cta-row">
                  <button class="btn-primary" @click="openQuoteFormWithProduct('Firewall SonicWall NS 5800', '212.000.000 VNĐ')">
                    Tư vấn giải pháp bảo mật
                  </button>
                </div>
              </div>
            </div>
          </div>
        </section>

        <!-- ================= TAB 4: BÁO GIÁ (QUOTATION - TRỌNG TÂM) ================= -->
        <section v-show="activeTab === 'quotation'" class="view-section">
          <!-- ATSolar Style Subpage Header Banner -->
          <div class="subpage-hero-banner">
            <div class="subpage-banner-container">
              <div class="subpage-breadcrumb">
                <a @click="switchTab('home')">Trang chủ</a>
                <span class="crumb-sep">/</span>
                <span class="crumb-current">Báo giá</span>
              </div>
              <h1 class="subpage-main-heading">BÁO GIÁ & DỰ TOÁN</h1>
            </div>
          </div>

          <!-- Interactive Quotation Estimator Tool -->
          <div class="estimator-calculator-card">
            <div class="estimator-header">
              <div class="estimator-title-group">
                <h3><Calculator :size="20" style="color: var(--primary-color);" /> Bảng Tính Dự Toán Trực Tuyến</h3>
                <p>Tùy chọn số lượng thiết bị và các dịch vụ kỹ thuật cần triển khai:</p>
              </div>
              <div class="estimator-total-badge">
                <span class="total-label">Tổng dự toán ước tính:</span>
                <span class="total-amount">{{ formatCurrency(estimatedTotal) }}</span>
              </div>
            </div>

            <div class="estimator-table-wrapper">
              <table class="estimator-table">
                <thead>
                  <tr>
                    <th style="width: 50px;">Chọn</th>
                    <th>Tên thiết bị / Phần mềm</th>
                    <th style="width: 170px;">Đơn giá niêm yết</th>
                    <th style="width: 140px; text-align: center;">Số lượng</th>
                    <th style="width: 180px; text-align: right;">Thành tiền</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="item in estimatorItems" :key="item.id" :class="{ 'row-active': item.selected && item.qty > 0 }">
                    <td>
                      <input type="checkbox" v-model="item.selected" class="estimator-checkbox" />
                    </td>
                    <td>
                      <strong>{{ item.name }}</strong>
                    </td>
                    <td class="price-col">{{ formatCurrency(item.price) }}</td>
                    <td>
                      <div class="qty-stepper" :class="{ 'stepper-disabled': !item.selected }">
                        <button class="qty-btn" @click="decreaseQty(item)" :disabled="!item.selected || item.qty <= 0">-</button>
                        <span class="qty-val">{{ item.qty }}</span>
                        <button class="qty-btn" @click="increaseQty(item)" :disabled="!item.selected">+</button>
                      </div>
                    </td>
                    <td class="total-col">{{ formatCurrency(item.selected ? item.price * item.qty : 0) }}</td>
                  </tr>
                </tbody>
              </table>
            </div>

            <!-- Additional Service Options -->
            <div class="estimator-options-row">
              <label class="option-check-item">
                <input type="checkbox" v-model="includeInstallation" />
                <span>Bao gồm gói khảo sát, lắp đặt & cấu hình On-site chuyên nghiệp (+5% dự toán)</span>
              </label>
              <label class="option-check-item">
                <input type="checkbox" v-model="includeExtendedWarranty" />
                <span>Gói bảo hành vàng mở rộng 24/7 thay thế linh kiện tận nơi (+8%)</span>
              </label>
            </div>

            <!-- Estimator Action Bar -->
            <div class="estimator-footer">
              <div class="estimator-note">
                * Lưu ý: Mức giá trên là giá niêm yết tham khảo. Đối với số lượng lớn hoặc dự án tích hợp, Đại Phúc có chính sách chiết khấu từ <strong>5% đến 20%</strong>.
              </div>
              <button class="btn-primary" @click="submitQuotationEstimate">
                <Send :size="16" />
                <span>Gửi yêu cầu nhận báo giá chiết khấu dự án</span>
              </button>
            </div>
          </div>

          <!-- Product Price Catalog with Filter -->
          <div class="section-spacer">
            <div class="product-catalog-header">
              <h2 class="catalog-title">Bảng Giá Chi Tiết Từng Danh Mục</h2>
              
              <div class="category-tabs">
                <button 
                  class="category-tab" 
                  :class="{ active: selectedQuotationCategory === 'all' }"
                  @click="selectedQuotationCategory = 'all'"
                >
                  Tất cả thiết bị
                </button>
                <button 
                  class="category-tab" 
                  :class="{ active: selectedQuotationCategory === 'switch' }"
                  @click="selectedQuotationCategory = 'switch'"
                >
                  Switch Chuyển Mạch
                </button>
                <button 
                  class="category-tab" 
                  :class="{ active: selectedQuotationCategory === 'firewall' }"
                  @click="selectedQuotationCategory = 'firewall'"
                >
                  Tường Lửa Firewall
                </button>
                <button 
                  class="category-tab" 
                  :class="{ active: selectedQuotationCategory === 'inverter' }"
                  @click="selectedQuotationCategory = 'inverter'"
                >
                  Bộ Biến Tần Inverter
                </button>
                <button 
                  class="category-tab" 
                  :class="{ active: selectedQuotationCategory === 'laptop' }"
                  @click="selectedQuotationCategory = 'laptop'"
                >
                  Laptop Doanh Nghiệp
                </button>
                <button 
                  class="category-tab" 
                  :class="{ active: selectedQuotationCategory === 'pc' }"
                  @click="selectedQuotationCategory = 'pc'"
                >
                  Máy Tính Để Bàn
                </button>
                <button 
                  class="category-tab" 
                  :class="{ active: selectedQuotationCategory === 'monitor' }"
                  @click="selectedQuotationCategory = 'monitor'"
                >
                  Màn Hình Máy Tính
                </button>
              </div>
            </div>

            <div class="hardware-grid">
              <div v-for="product in filteredProducts" :key="product.id" class="hardware-card">
                <div v-if="product.image" class="hw-card-image-wrapper">
                  <img :src="product.image" :alt="product.name" class="hw-card-image" loading="lazy" />
                </div>
                
                <div class="hw-card-meta">
                  <span class="hw-category-tag">{{ product.categoryName }}</span>
                  <span class="hw-price-tag">{{ product.price }}</span>
                </div>

                <h3 class="hw-card-title">{{ product.name }}</h3>
                
                <div class="hw-card-desc" v-html="formatDescription(product.description)"></div>
                
                <div class="hw-card-actions">
                  <button 
                    class="btn-primary btn-full" 
                    @click="openQuoteFormWithProduct(product.name, product.price)"
                  >
                    Nhận báo giá sản phẩm này
                  </button>
                </div>
              </div>
            </div>
          </div>
        </section>

        <!-- ================= TAB 5: DỰ ÁN (PROJECTS) ================= -->
        <section v-show="activeTab === 'projects'" class="view-section">
          <!-- ATSolar Style Subpage Header Banner -->
          <div class="subpage-hero-banner">
            <div class="subpage-banner-container">
              <div class="subpage-breadcrumb">
                <a @click="switchTab('home')">Trang chủ</a>
                <span class="crumb-sep">/</span>
                <span class="crumb-current">Dự án</span>
              </div>
              <h1 class="subpage-main-heading">CÁC DỰ ÁN TIÊU BIỂU</h1>
            </div>
          </div>

          <div class="projects-grid">
            <div v-for="project in projectsList" :key="project.id" class="project-card">
              <div class="project-image-box">
                <img :src="project.image" :alt="project.title" />
                <span class="project-year-badge">{{ project.year }}</span>
              </div>
              <div class="project-body">
                <span class="project-cat">{{ project.category }}</span>
                <h3 class="project-title">{{ project.title }}</h3>
                <div class="project-client"><strong>Khách hàng:</strong> {{ project.client }}</div>
                <p class="project-desc">{{ project.desc }}</p>
                <button class="btn-outline" @click="showContactModal = true; formMsg = `Tôi muốn tham khảo giải pháp tương tự dự án: ${project.title}`">
                  Tư vấn dự án tương tự
                </button>
              </div>
            </div>
          </div>
        </section>

        <!-- ================= TAB 6: TIN TỨC (BLOG) ================= -->
        <section v-show="activeTab === 'blog'" class="view-section">
          <!-- ATSolar Style Subpage Header Banner -->
          <div class="subpage-hero-banner">
            <div class="subpage-banner-container">
              <div class="subpage-breadcrumb">
                <a @click="switchTab('home')">Trang chủ</a>
                <span class="crumb-sep">/</span>
                <span class="crumb-current">Tin tức</span>
              </div>
              <h1 class="subpage-main-heading">TIN TỨC & BÀI VIẾT</h1>
            </div>
          </div>

          <div class="blog-grid">
            <div v-for="post in blogPosts" :key="post.id" class="blog-card">
              <div class="blog-meta-row">
                <span class="blog-tag">{{ post.category }}</span>
                <span class="blog-date"><Calendar :size="13" /> {{ post.date }}</span>
              </div>
              <h3 class="blog-title">{{ post.title }}</h3>
              <p class="blog-summary">{{ post.summary }}</p>
              <button class="btn-outline" style="align-self: flex-start; margin-top: auto;" @click="showContactModal = true; formMsg = `Xin tư vấn về chủ đề: ${post.title}`">
                Đọc thêm & Nhận tư vấn
              </button>
            </div>
          </div>
        </section>

        <!-- ================= TAB 7: LIÊN HỆ (CONTACT - TRỌNG TÂM) ================= -->
        <section v-show="activeTab === 'contact'" class="view-section">
          <!-- ATSolar Style Subpage Header Banner -->
          <div class="subpage-hero-banner">
            <div class="subpage-banner-container">
              <div class="subpage-breadcrumb">
                <a @click="switchTab('home')">Trang chủ</a>
                <span class="crumb-sep">/</span>
                <span class="crumb-current">Liên hệ</span>
              </div>
              <h1 class="subpage-main-heading">LIÊN HỆ VỚI CHÚNG TÔI</h1>
            </div>
          </div>

          <div class="contact-page-layout">
            <!-- Contact Details -->
            <div class="contact-info-panel">
              <div class="contact-info-card">
                <h3>Thông Tin Doanh Nghiệp</h3>
                <p class="company-name">CÔNG TY TNHH TM DV KỸ THUẬT ĐẠI PHÚC</p>
                
                <div class="contact-items-list">
                  <div class="contact-item-row">
                    <div class="contact-icon"><MapPin :size="20" /></div>
                    <div>
                      <strong>Trụ sở chính:</strong>
                      <p>Khu vực TP. Hồ Chí Minh & Chi nhánh toàn quốc</p>
                    </div>
                  </div>

                  <div class="contact-item-row">
                    <div class="contact-icon"><Phone :size="20" /></div>
                    <div>
                      <strong>Hotline tư vấn 24/7:</strong>
                      <p><a href="tel:0355331720" style="color: var(--primary-color); font-weight: 700; text-decoration: none;">0355.331.720</a></p>
                    </div>
                  </div>

                  <div class="contact-item-row">
                    <div class="contact-icon"><Mail :size="20" /></div>
                    <div>
                      <strong>Email tiếp nhận:</strong>
                      <p><a href="mailto:contact@daiphuc.vn" style="color: var(--text-color); text-decoration: none;">contact@daiphuc.vn</a></p>
                    </div>
                  </div>

                  <div class="contact-item-row">
                    <div class="contact-icon"><Clock :size="20" /></div>
                    <div>
                      <strong>Thời gian làm việc:</strong>
                      <p>Thứ Hai - Thứ Bảy: 08:00 - 17:30 (Kỹ thuật hỗ trợ 24/7)</p>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Service commitments -->
              <div class="commitment-card">
                <h4>Cam Kết Từ Kỹ Thuật Đại Phúc</h4>
                <ul class="commitment-list">
                  <li><CheckCircle :size="16" style="color: #059669;" /> Báo giá minh bạch, cạnh tranh nhất thị trường.</li>
                  <li><CheckCircle :size="16" style="color: #059669;" /> 100% Thiết bị chính hãng có đầy đủ chứng chỉ CO/CQ.</li>
                  <li><CheckCircle :size="16" style="color: #059669;" /> Đội ngũ kỹ sư được chứng chỉ hãng trực tiếp thi công.</li>
                </ul>
              </div>
            </div>

            <!-- Contact Form -->
            <div class="contact-form-panel">
              <div class="contact-form-card">
                <span class="modal-header-tag">Gửi lời nhắn trực tuyến</span>
                <h3 class="form-card-title">Yêu Cầu Báo Giá & Tư Vấn</h3>

                <form v-if="!submitSuccess" class="contact-form-content" @submit.prevent="submitContactForm">
                  <div class="form-row">
                    <div class="form-item">
                      <label class="form-item-label" for="cname">Họ và tên *</label>
                      <input 
                        id="cname" 
                        type="text" 
                        v-model="formName" 
                        required 
                        placeholder="Ví dụ: Nguyễn Văn A" 
                        class="form-item-input"
                      />
                    </div>
                    <div class="form-item">
                      <label class="form-item-label" for="cphone">Số điện thoại *</label>
                      <input 
                        id="cphone" 
                        type="tel" 
                        v-model="formPhone" 
                        required 
                        placeholder="Ví dụ: 0901 234 567" 
                        class="form-item-input"
                      />
                    </div>
                  </div>

                  <div class="form-row">
                    <div class="form-item">
                      <label class="form-item-label" for="cemail">Địa chỉ Email *</label>
                      <input 
                        id="cemail" 
                        type="email" 
                        v-model="formEmail" 
                        required 
                        placeholder="name@company.com" 
                        class="form-item-input"
                      />
                    </div>
                    <div class="form-item">
                      <label class="form-item-label" for="cservice">Nhu cầu quan tâm</label>
                      <select id="cservice" v-model="formService" class="form-item-input">
                        <option value="Tư vấn báo giá thiết bị Switch">Tư vấn báo giá thiết bị Switch</option>
                        <option value="Tư vấn Tường lửa SonicWall">Tư vấn Tường lửa SonicWall</option>
                        <option value="Tư vấn Biến tần Inverter 3KVA">Tư vấn Biến tần Inverter 3KVA</option>
                        <option value="Tư vấn Laptop ASUS ExpertBook">Tư vấn Laptop ASUS ExpertBook</option>
                        <option value="Tư vấn Máy tính để bàn ASUS ExpertCenter D700 SFF">Tư vấn Máy tính để bàn ASUS ExpertCenter D700 SFF</option>
                        <option value="Tư vấn Máy tính để bàn ASUS ExpertCenter D701 SFF">Tư vấn Máy tính để bàn ASUS ExpertCenter D701 SFF</option>
                        <option value="Tư vấn Máy tính để bàn ASUS ExpertCenter P500 SFF">Tư vấn Máy tính để bàn ASUS ExpertCenter P500 SFF</option>
                        <option value="Tư vấn Màn hình ASUS VY249HGR-R">Tư vấn Màn hình ASUS VY249HGR-R</option>
                        <option value="Tư vấn Phần mềm Station Monitor">Tư vấn Phần mềm Station Monitor</option>
                        <option value="Khảo sát giải pháp trọn gói">Khảo sát giải pháp trọn gói</option>
                        <option value="Hợp tác kinh doanh & Khác">Hợp tác kinh doanh & Khác</option>
                      </select>
                    </div>
                  </div>

                  <div class="form-item">
                    <label class="form-item-label" for="cmsg">Chi tiết yêu cầu / Thông tin dự án</label>
                    <textarea 
                      id="cmsg" 
                      v-model="formMsg" 
                      required 
                      placeholder="Mô tả nhu cầu số lượng, địa điểm lắp đặt hoặc các yêu cầu kỹ thuật cần hỗ trợ..." 
                      class="form-item-input"
                    ></textarea>
                  </div>

                  <button type="submit" class="btn-primary btn-full" :disabled="isSubmitting">
                    <span v-if="!isSubmitting">Gửi yêu cầu báo giá ngay</span>
                    <span v-else>Đang xử lý gửi thông tin...</span>
                  </button>
                </form>

                <div v-else class="modal-success-state">
                  <CheckCircle :size="52" class="success-check-icon" />
                  <h4>Gửi yêu cầu thành công!</h4>
                  <p>Cảm ơn bạn đã liên hệ với Đại Phúc. Chuyên viên kinh doanh và kỹ thuật sẽ chủ động liên hệ lại với bạn trong vòng 30 phút.</p>
                </div>
              </div>
            </div>
          </div>
        </section>

      </div>
    </main>

    <!-- Floating Circular Action Icons (ATSolar style: Zalo, Phone, Facebook) -->
    <div class="ats-floating-socials">
      <a href="https://zalo.me/0355331720" target="_blank" class="ats-social-circle zalo" aria-label="Zalo">
        <span>Zalo</span>
      </a>
      <a href="tel:0355331720" class="ats-social-circle phone" aria-label="Hotline">
        <Phone :size="22" />
      </a>
      <a href="#" class="ats-social-circle facebook" aria-label="Facebook">
        <Facebook :size="22" />
      </a>
    </div>

    <!-- Corporate Footer -->
    <footer class="site-footer">
      <div class="container">
        <div class="footer-top-grid">
          <div>
            <div class="brand-logo-link" style="margin-bottom: 14px;">
              <img src="/logo.png?v=2" alt="Đại Phúc Logo" style="height: 38px; filter: brightness(1.2);" />
              <div class="logo-text-group">
                <span class="logo-brand-name" style="color: #FFFFFF;">ĐẠI PHÚC</span>
                <span class="logo-slogan" style="color: #94A3B8;">Giải pháp tối ưu, Khởi nguyên thịnh vượng</span>
              </div>
            </div>
            <p style="font-size: 13px; line-height: 1.6; color: #94A3B8; margin-bottom: 16px;">
              Công ty TNHH TM DV Kỹ thuật Đại Phúc là đối tác uy tín trong việc cung cấp các giải pháp công nghệ thông tin, thiết bị chuyển mạch mạng, tường lửa bảo mật và phần mềm giám sát quan trắc chuyên dụng.
            </p>
          </div>

          <div>
            <h4 class="footer-col-title">Liên Kết Nhanh</h4>
            <ul class="footer-links-list">
              <li><a class="footer-link-item" @click="switchTab('home')">Trang chủ</a></li>
              <li><a class="footer-link-item" @click="switchTab('about')">Giới thiệu công ty</a></li>
              <li><a class="footer-link-item" @click="switchTab('solutions')">Giải pháp kỹ thuật</a></li>
              <li><a class="footer-link-item" @click="switchTab('quotation')">Bảng giá & Dự toán</a></li>
              <li><a class="footer-link-item" @click="switchTab('projects')">Dự án đã thực hiện</a></li>
              <li><a class="footer-link-item" @click="switchTab('contact')">Liên hệ & Tư vấn</a></li>
            </ul>
          </div>

          <div>
            <h4 class="footer-col-title">Sản Phẩm Chủ Lực</h4>
            <ul class="footer-links-list">
              <li><a class="footer-link-item" @click="switchTab('quotation')">Switch Extreme X435-24T</a></li>
              <li><a class="footer-link-item" @click="switchTab('quotation')">Switch Extreme X435-24P</a></li>
              <li><a class="footer-link-item" @click="switchTab('quotation')">Firewall SonicWall NS 5800</a></li>
              <li><a class="footer-link-item" @click="switchTab('quotation')">Switch Juniper EX4100-24P</a></li>
              <li><a class="footer-link-item" @click="switchTab('quotation')">Inverter 3KVA-110VDC/220VAC</a></li>
              <li><a class="footer-link-item" @click="switchTab('solutions')">Phần mềm Station Monitor</a></li>
            </ul>
          </div>

          <div>
            <h4 class="footer-col-title">Thông Tin Liên Hệ</h4>
            <div class="footer-contact-item">
              <MapPin :size="16" style="color: #00A8C6; flex-shrink: 0; margin-top: 2px;" />
              <span>TP. Hồ Chí Minh, Việt Nam</span>
            </div>
            <div class="footer-contact-item">
              <Phone :size="16" style="color: #00A8C6; flex-shrink: 0; margin-top: 2px;" />
              <span>Hotline: 0355.331.720</span>
            </div>
            <div class="footer-contact-item">
              <Mail :size="16" style="color: #00A8C6; flex-shrink: 0; margin-top: 2px;" />
              <span>Email: contact@daiphuc.vn</span>
            </div>
            <div class="footer-contact-item">
              <Clock :size="16" style="color: #00A8C6; flex-shrink: 0; margin-top: 2px;" />
              <span>T2 - T7: 08:00 - 17:30</span>
            </div>
          </div>
        </div>

        <div class="footer-bottom-bar">
          <div>
            © 2026 Công ty TNHH TM DV Kỹ thuật Đại Phúc. Tất cả quyền được bảo lưu.
          </div>
          <div class="footer-social-links">
            <a href="#" class="footer-social-btn" aria-label="Facebook"><Facebook :size="18" /></a>
            <a href="#" class="footer-social-btn" aria-label="LinkedIn"><Linkedin :size="18" /></a>
            <a href="#" class="footer-social-btn" aria-label="YouTube"><Youtube :size="18" /></a>
          </div>
        </div>
      </div>
    </footer>

    <!-- Toast Notification for Downloads -->
    <transition name="fade">
      <div v-if="downloadSuccess" class="download-toast-sticky">
        <CheckCircle :size="18" style="color: #34D399;" />
        <span>Bắt đầu tải xuống {{ downloadedFile }}</span>
      </div>
    </transition>

    <!-- Modal Popup for Instant Quotes / Contact -->
    <transition name="fade">
      <div v-if="showContactModal" class="modal-backdrop" @click.self="showContactModal = false">
        <div class="modal-content-card">
          <button class="close-modal-trigger" @click="showContactModal = false" aria-label="Close modal">
            <X :size="20" />
          </button>
          
          <span class="modal-header-tag">Liên hệ báo giá</span>
          <h3 class="modal-main-title">{{ formService }}</h3>
          
          <form v-if="!submitSuccess" class="contact-form-content" @submit.prevent="submitContactForm">
            <div class="form-row">
              <div class="form-item">
                <label class="form-item-label" for="modalName">Họ và tên *</label>
                <input 
                  id="modalName" 
                  type="text" 
                  v-model="formName" 
                  required 
                  placeholder="Ví dụ: Nguyễn Văn A" 
                  class="form-item-input"
                />
              </div>
              <div class="form-item">
                <label class="form-item-label" for="modalPhone">Số điện thoại *</label>
                <input 
                  id="modalPhone" 
                  type="tel" 
                  v-model="formPhone" 
                  required 
                  placeholder="Ví dụ: 0901 234 567" 
                  class="form-item-input"
                />
              </div>
            </div>
            
            <div class="form-item">
              <label class="form-item-label" for="modalEmail">Địa chỉ Email *</label>
              <input 
                id="modalEmail" 
                type="email" 
                v-model="formEmail" 
                required 
                placeholder="name@company.com" 
                class="form-item-input"
              />
            </div>
            
            <div class="form-item">
              <label class="form-item-label" for="modalMsg">Thông tin chi tiết yêu cầu</label>
              <textarea 
                id="modalMsg" 
                v-model="formMsg" 
                required 
                class="form-item-input"
                style="min-height: 100px;"
              ></textarea>
            </div>
            
            <button type="submit" class="btn-primary btn-full" :disabled="isSubmitting">
              <span v-if="!isSubmitting">Gửi thông tin nhận báo giá</span>
              <span v-else>Đang gửi dữ liệu...</span>
            </button>
          </form>
          
          <div v-else class="modal-success-state">
            <CheckCircle :size="48" class="success-check-icon" />
            <h4>Gửi thành công!</h4>
            <p>Chúng tôi đã tiếp nhận yêu cầu và sẽ gửi báo giá chi tiết trong thời gian sớm nhất.</p>
          </div>
        </div>
      </div>
    </transition>

    <!-- Search Modal / Overlay (ATSolar Style) -->
    <transition name="fade">
      <div v-if="showSearchModal" class="modal-backdrop" @click.self="showSearchModal = false">
        <div class="search-modal-card">
          <div class="search-modal-header">
            <Search :size="20" class="search-input-icon" />
            <input 
              ref="searchInputRef"
              type="text" 
              v-model="searchQuery" 
              placeholder="Tìm kiếm Switch, Firewall, Inverter, giải pháp, dự án..."
              class="search-modal-input"
            />
            <button v-if="searchQuery" class="search-clear-btn" @click="searchQuery = ''" aria-label="Clear search">
              <X :size="16" />
            </button>
            <button class="close-search-btn" @click="showSearchModal = false" aria-label="Close search">
              ESC
            </button>
          </div>

          <!-- Quick Suggestion Tags when query is empty -->
          <div v-if="!searchQuery.trim()" class="search-suggestions">
            <div class="search-section-label">GỢI Ý TÌM KIẾM PHỔ BIẾN</div>
            <div class="search-tags-row">
              <button class="search-tag-chip" @click="searchQuery = 'Switch Extreme'">Switch Extreme</button>
              <button class="search-tag-chip" @click="searchQuery = 'Juniper'">Switch Juniper</button>
              <button class="search-tag-chip" @click="searchQuery = 'SonicWall'">Firewall SonicWall</button>
              <button class="search-tag-chip" @click="searchQuery = 'Inverter'">Inverter 3KVA</button>
              <button class="search-tag-chip" @click="searchQuery = 'ExpertBook'">ASUS ExpertBook</button>
              <button class="search-tag-chip" @click="searchQuery = 'ExpertCenter'">ASUS ExpertCenter</button>
              <button class="search-tag-chip" @click="searchQuery = 'VY249HGR'">Màn hình ASUS 120Hz</button>
              <button class="search-tag-chip" @click="searchQuery = 'Station Monitor'">Station Monitor</button>
            </div>
          </div>

          <!-- Search Results List -->
          <div v-else class="search-results-container">
            <div v-if="totalSearchResults === 0" class="search-empty-state">
              <HelpCircle :size="36" style="color: #94A3B8; margin-bottom: 8px;" />
              <p>Không tìm thấy kết quả phù hợp cho "<strong>{{ searchQuery }}</strong>"</p>
              <span>Vui lòng thử từ khóa khác như Switch, Firewall, Inverter hoặc SCADA</span>
            </div>

            <div v-else class="search-results-groups">
              <!-- Products Results -->
              <div v-if="searchResults.products.length > 0" class="search-group">
                <div class="search-section-label">SẢN PHẨM & THIẾT BỊ ({{ searchResults.products.length }})</div>
                <div 
                  v-for="p in searchResults.products" 
                  :key="p.id" 
                  class="search-result-item"
                  @click="handleSearchResultClick('product', p)"
                >
                  <img :src="p.image" :alt="p.name" class="search-item-thumb" />
                  <div class="search-item-info">
                    <div class="search-item-title">{{ p.name }}</div>
                    <div class="search-item-meta">{{ p.categoryName }} • <span class="search-item-price">{{ p.price }}</span></div>
                  </div>
                  <ChevronRight :size="16" class="search-item-arrow" />
                </div>
              </div>

              <!-- Projects Results -->
              <div v-if="searchResults.projects.length > 0" class="search-group">
                <div class="search-section-label">DỰ ÁN TIÊU BIỂU ({{ searchResults.projects.length }})</div>
                <div 
                  v-for="proj in searchResults.projects" 
                  :key="proj.id" 
                  class="search-result-item"
                  @click="handleSearchResultClick('project', proj)"
                >
                  <img :src="proj.image" :alt="proj.title" class="search-item-thumb" />
                  <div class="search-item-info">
                    <div class="search-item-title">{{ proj.title }}</div>
                    <div class="search-item-meta">{{ proj.client }} • {{ proj.category }}</div>
                  </div>
                  <ChevronRight :size="16" class="search-item-arrow" />
                </div>
              </div>

              <!-- Blog Results -->
              <div v-if="searchResults.blogs.length > 0" class="search-group">
                <div class="search-section-label">TIN TỨC & BÀI VIẾT ({{ searchResults.blogs.length }})</div>
                <div 
                  v-for="b in searchResults.blogs" 
                  :key="b.id" 
                  class="search-result-item"
                  @click="handleSearchResultClick('blog', b)"
                >
                  <div class="search-item-icon-box">
                    <FileText :size="18" />
                  </div>
                  <div class="search-item-info">
                    <div class="search-item-title">{{ b.title }}</div>
                    <div class="search-item-meta">{{ b.category }} • {{ b.date }}</div>
                  </div>
                  <ChevronRight :size="16" class="search-item-arrow" />
                </div>
              </div>
            </div>
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

.section-spacer {
  margin-top: 56px;
}

/* Feature Grid (4 cols) */
.grid-4-cols {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
}

@media (max-width: 1024px) {
  .grid-4-cols {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 580px) {
  .grid-4-cols {
    grid-template-columns: 1fr;
  }
}

.solution-feature-box {
  background-color: #FFFFFF;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-lg);
  padding: 24px 20px;
  display: flex;
  flex-direction: column;
  cursor: pointer;
  transition: all var(--transition-normal);
  box-shadow: var(--shadow-xs);
}

.solution-feature-box:hover {
  border-color: var(--accent-color);
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
}

.solution-icon {
  width: 52px;
  height: 52px;
  border-radius: var(--radius-md);
  background-color: var(--primary-light);
  color: var(--primary-color);
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 16px;
}

.solution-feature-box h3 {
  font-size: 16px;
  font-weight: 700;
  color: var(--text-color);
  margin-bottom: 8px;
}

.solution-feature-box p {
  font-size: 13px;
  color: var(--text-muted);
  line-height: 1.5;
  margin-bottom: 16px;
  flex-grow: 1;
}

.solution-link {
  font-size: 13px;
  font-weight: 700;
  color: var(--accent-color);
  display: inline-flex;
  align-items: center;
  gap: 4px;
}

/* Hardware Card in Grid */
.hardware-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(340px, 1fr));
  gap: 24px;
  width: 100%;
}

.hardware-card {
  background: #FFFFFF;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-lg);
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 14px;
  transition: all var(--transition-normal);
  box-shadow: var(--shadow-xs);
}

.hardware-card:hover {
  transform: translateY(-3px);
  box-shadow: var(--shadow-lg);
  border-color: var(--border-hover);
}

.hw-card-image-wrapper {
  width: 100%;
  height: 200px;
  border-radius: var(--radius-md);
  overflow: hidden;
  background-color: #F8FAFC;
  border: 1px solid var(--border-color);
  display: flex;
  align-items: center;
  justify-content: center;
}

.hw-card-image {
  width: 100%;
  height: 100%;
  object-fit: contain;
  padding: 10px;
  transition: transform var(--transition-normal);
}

.hardware-card:hover .hw-card-image {
  transform: scale(1.03);
}

.hw-card-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.hw-category-tag {
  font-size: 11px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.06em;
  color: var(--accent-color);
  background-color: var(--accent-light);
  padding: 4px 8px;
  border-radius: var(--radius-sm);
}

.hw-price-tag {
  background-color: #0F172A;
  color: #F8FAFC;
  padding: 4px 10px;
  border-radius: var(--radius-sm);
  font-weight: 700;
  font-size: 13px;
}

.hw-card-title {
  font-size: 17px;
  font-weight: 700;
  color: var(--text-color);
  line-height: 1.35;
}

.hw-card-desc {
  font-size: 13px;
  color: var(--text-muted);
  line-height: 1.6;
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.hw-card-desc :deep(.hw-desc-paragraph) {
  margin: 0;
  font-weight: 500;
  color: var(--text-color);
}

.hw-card-desc :deep(.hw-feature-list) {
  margin: 0;
  padding-left: 18px;
  list-style-type: square;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.hw-card-desc :deep(.hw-feature-list li) {
  padding-left: 2px;
  color: var(--text-muted);
}

.hw-card-actions {
  margin-top: auto;
  padding-top: 8px;
}

/* Estimator Calculator Card */
.estimator-calculator-card {
  background: #FFFFFF;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-xl);
  padding: 32px;
  box-shadow: var(--shadow-md);
  margin-bottom: 48px;
}

.estimator-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-bottom: 20px;
  border-bottom: 1px solid var(--border-color);
  margin-bottom: 24px;
}

.estimator-title-group h3 {
  font-size: 20px;
  font-weight: 800;
  color: var(--text-color);
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 4px;
}

.estimator-title-group p {
  font-size: 14px;
  color: var(--text-muted);
}

.estimator-total-badge {
  background-color: var(--primary-light);
  border: 1px solid rgba(30, 64, 175, 0.2);
  border-radius: var(--radius-md);
  padding: 12px 20px;
  text-align: right;
}

.total-label {
  font-size: 11px;
  font-weight: 700;
  text-transform: uppercase;
  color: var(--text-muted);
  display: block;
  letter-spacing: 0.05em;
}

.total-amount {
  font-size: 22px;
  font-weight: 800;
  color: var(--primary-color);
}

@media (max-width: 768px) {
  .estimator-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 16px;
  }
  .estimator-total-badge {
    width: 100%;
    text-align: left;
  }
}

.estimator-table-wrapper {
  overflow-x: auto;
  margin-bottom: 20px;
}

.estimator-table {
  width: 100%;
  border-collapse: collapse;
  text-align: left;
  font-size: 14px;
}

.estimator-table th {
  background-color: var(--bg-subtle);
  padding: 12px 14px;
  font-size: 12px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  color: var(--text-muted);
  border-bottom: 1px solid var(--border-color);
}

.estimator-table td {
  padding: 14px;
  border-bottom: 1px solid var(--border-color);
  vertical-align: middle;
}

.estimator-table tr.row-active {
  background-color: #F8FAFC;
}

.estimator-checkbox {
  width: 18px;
  height: 18px;
  cursor: pointer;
  accent-color: var(--primary-color);
}

.price-col {
  color: var(--text-muted);
}

.total-col {
  font-weight: 700;
  color: var(--text-color);
  text-align: right;
}

.qty-stepper {
  display: inline-flex;
  align-items: center;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-sm);
  background-color: #FFFFFF;
}

.qty-btn {
  background: none;
  border: none;
  padding: 4px 10px;
  font-weight: 700;
  cursor: pointer;
  color: var(--text-color);
}

.qty-btn:disabled {
  opacity: 0.3;
  cursor: not-allowed;
}

.qty-val {
  min-width: 24px;
  text-align: center;
  font-weight: 600;
}

.stepper-disabled {
  opacity: 0.5;
}

.estimator-options-row {
  display: flex;
  flex-direction: column;
  gap: 12px;
  background-color: var(--bg-subtle);
  padding: 16px 20px;
  border-radius: var(--radius-md);
  margin-bottom: 24px;
}

.option-check-item {
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 14px;
  font-weight: 600;
  color: var(--text-color);
  cursor: pointer;
}

.option-check-item input {
  width: 18px;
  height: 18px;
  accent-color: var(--primary-color);
}

.estimator-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 20px;
  flex-wrap: wrap;
}

.estimator-note {
  font-size: 13px;
  color: var(--text-muted);
  max-width: 600px;
}

/* Product Catalog Header */
.product-catalog-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24px;
  flex-wrap: wrap;
  gap: 16px;
}

.catalog-title {
  font-size: 22px;
  font-weight: 800;
  color: var(--text-color);
}

/* Category Tabs */
.category-tabs {
  display: flex;
  gap: 6px;
  background-color: var(--bg-subtle);
  padding: 4px;
  border-radius: var(--radius-md);
  border: 1px solid var(--border-color);
  flex-wrap: wrap;
}

.category-tab {
  background: transparent;
  border: none;
  padding: 8px 16px;
  border-radius: var(--radius-sm);
  font-family: var(--font-sans);
  font-weight: 600;
  font-size: 13px;
  color: var(--text-muted);
  cursor: pointer;
  transition: all var(--transition-fast);
}

.category-tab.active {
  background-color: #FFFFFF;
  color: var(--primary-color);
  box-shadow: var(--shadow-xs);
  font-weight: 700;
}

/* Stats panel */
.hero-stats-panel {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
}

@media (max-width: 900px) {
  .hero-stats-panel {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 480px) {
  .hero-stats-panel {
    grid-template-columns: 1fr;
  }
}

.stat-card {
  background-color: #FFFFFF;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-md);
  padding: 20px 16px;
  text-align: center;
  transition: all var(--transition-fast);
  box-shadow: var(--shadow-xs);
}

.stat-card:hover {
  border-color: var(--primary-color);
  box-shadow: var(--shadow-sm);
  transform: translateY(-2px);
}

.stat-number {
  font-size: 28px;
  font-weight: 800;
  color: var(--primary-color);
  line-height: 1;
  margin-bottom: 6px;
}

.stat-label {
  font-size: 12px;
  font-weight: 600;
  color: var(--text-muted);
}

/* About / Workflow Styles */
.about-hero-card {
  background: #FFFFFF;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-xl);
  padding: 48px;
  box-shadow: var(--shadow-md);
  position: relative;
  overflow: hidden;
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.about-hero-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: linear-gradient(90deg, #12265B 0%, #00A8C6 100%);
}

.about-title {
  font-size: 30px;
  font-weight: 800;
  color: var(--text-color);
  line-height: 1.3;
  margin-bottom: 16px;
}

.about-desc {
  font-size: 15px;
  color: var(--text-muted);
  line-height: 1.75;
  margin-bottom: 32px;
  max-width: 800px;
}

.about-highlights-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 16px;
  width: 100%;
  margin-bottom: 40px;
  text-align: left;
}

.highlight-box {
  background-color: var(--bg-subtle);
  border: 1px solid var(--border-color);
  border-radius: var(--radius-md);
  padding: 18px;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.highlight-label {
  font-size: 11px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.06em;
  color: var(--text-muted);
}

.highlight-value {
  font-size: 14px;
  font-weight: 700;
  color: var(--text-color);
}

.workflow-section {
  width: 100%;
  border-top: 1px solid var(--border-color);
  padding-top: 36px;
  text-align: left;
}

.workflow-title {
  font-size: 22px;
  font-weight: 800;
  color: var(--text-color);
  text-align: center;
  margin-bottom: 28px;
}

.workflow-steps-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
}

@media (max-width: 900px) {
  .workflow-steps-grid {
    grid-template-columns: repeat(2, 1fr);
  }
  .about-highlights-grid {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 580px) {
  .workflow-steps-grid {
    grid-template-columns: 1fr;
  }
}

.step-card {
  background-color: var(--bg-subtle);
  border: 1px solid var(--border-color);
  border-radius: var(--radius-md);
  padding: 20px 16px;
}

.step-badge {
  display: inline-block;
  font-size: 11px;
  font-weight: 800;
  color: #FFFFFF;
  background-color: var(--primary-color);
  padding: 3px 8px;
  border-radius: var(--radius-sm);
  margin-bottom: 10px;
}

.step-card h4 {
  font-size: 15px;
  font-weight: 700;
  color: var(--text-color);
  margin-bottom: 8px;
}

.step-card p {
  font-size: 13px;
  color: var(--text-muted);
  line-height: 1.5;
}

/* Solutions List */
.solutions-list-stack {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.solution-detail-card {
  background: #FFFFFF;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-lg);
  padding: 36px;
  box-shadow: var(--shadow-sm);
  position: relative;
  overflow: hidden;
}

.solution-badge-category {
  font-size: 12px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.06em;
  color: var(--accent-color);
  margin-bottom: 8px;
}

.solution-detail-card h2 {
  font-size: 22px;
  font-weight: 800;
  color: var(--text-color);
  margin-bottom: 12px;
}

.solution-detail-card p {
  font-size: 14px;
  color: var(--text-muted);
  line-height: 1.7;
  margin-bottom: 18px;
}

.solution-bullet-points {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 8px;
  margin-bottom: 24px;
}

.solution-bullet-points li {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 14px;
  font-weight: 600;
  color: var(--text-color);
}

.solution-bullet-points li svg {
  color: var(--success-color);
  flex-shrink: 0;
}

.solution-cta-row {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
}

/* Projects Grid */
.projects-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}

@media (max-width: 900px) {
  .projects-grid {
    grid-template-columns: 1fr;
  }
}

.project-card {
  background: #FFFFFF;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-lg);
  overflow: hidden;
  box-shadow: var(--shadow-sm);
  display: flex;
  flex-direction: column;
}

.project-image-box {
  width: 100%;
  height: 200px;
  background-color: var(--bg-subtle);
  position: relative;
  overflow: hidden;
}

.project-image-box img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.project-year-badge {
  position: absolute;
  top: 12px;
  right: 12px;
  background-color: rgba(15, 23, 42, 0.85);
  color: #FFFFFF;
  padding: 4px 10px;
  border-radius: var(--radius-sm);
  font-size: 12px;
  font-weight: 700;
}

.project-body {
  padding: 24px;
  display: flex;
  flex-direction: column;
  flex-grow: 1;
}

.project-cat {
  font-size: 11px;
  font-weight: 700;
  text-transform: uppercase;
  color: var(--accent-color);
  margin-bottom: 6px;
}

.project-title {
  font-size: 17px;
  font-weight: 700;
  color: var(--text-color);
  line-height: 1.4;
  margin-bottom: 8px;
}

.project-client {
  font-size: 13px;
  color: var(--text-muted);
  margin-bottom: 12px;
}

.project-desc {
  font-size: 13px;
  color: var(--text-muted);
  line-height: 1.6;
  margin-bottom: 20px;
  flex-grow: 1;
}

/* Blog Grid */
.blog-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}

@media (max-width: 900px) {
  .blog-grid {
    grid-template-columns: 1fr;
  }
}

.blog-card {
  background: #FFFFFF;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-lg);
  padding: 24px;
  box-shadow: var(--shadow-sm);
  display: flex;
  flex-direction: column;
}

.blog-meta-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.blog-tag {
  font-size: 11px;
  font-weight: 700;
  text-transform: uppercase;
  color: var(--primary-color);
  background-color: var(--primary-light);
  padding: 3px 8px;
  border-radius: var(--radius-sm);
}

.blog-date {
  font-size: 12px;
  color: var(--text-muted);
  display: flex;
  align-items: center;
  gap: 4px;
}

.blog-title {
  font-size: 17px;
  font-weight: 700;
  color: var(--text-color);
  line-height: 1.4;
  margin-bottom: 10px;
}

.blog-summary {
  font-size: 13px;
  color: var(--text-muted);
  line-height: 1.6;
  margin-bottom: 20px;
}

/* Contact Page Layout */
.contact-page-layout {
  display: grid;
  grid-template-columns: 1fr 1.3fr;
  gap: 32px;
  align-items: flex-start;
}

@media (max-width: 900px) {
  .contact-page-layout {
    grid-template-columns: 1fr;
  }
}

.contact-info-panel {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.contact-info-card, .commitment-card {
  background: #FFFFFF;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-lg);
  padding: 28px;
  box-shadow: var(--shadow-sm);
}

.contact-info-card h3 {
  font-size: 18px;
  font-weight: 800;
  color: var(--text-color);
  margin-bottom: 4px;
}

.company-name {
  font-size: 12px;
  font-weight: 700;
  color: var(--accent-color);
  letter-spacing: 0.05em;
  margin-bottom: 24px;
}

.contact-items-list {
  display: flex;
  flex-direction: column;
  gap: 18px;
}

.contact-item-row {
  display: flex;
  align-items: flex-start;
  gap: 14px;
}

.contact-icon {
  width: 40px;
  height: 40px;
  border-radius: var(--radius-md);
  background-color: var(--primary-light);
  color: var(--primary-color);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.contact-item-row strong {
  font-size: 13px;
  color: var(--text-color);
  display: block;
  margin-bottom: 2px;
}

.contact-item-row p {
  font-size: 14px;
  color: var(--text-muted);
  margin: 0;
}

.commitment-card h4 {
  font-size: 15px;
  font-weight: 700;
  color: var(--text-color);
  margin-bottom: 12px;
}

.commitment-list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.commitment-list li {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 13px;
  color: var(--text-muted);
  line-height: 1.5;
}

.contact-form-card {
  background: #FFFFFF;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-lg);
  padding: 32px;
  box-shadow: var(--shadow-md);
}

.form-card-title {
  font-size: 22px;
  font-weight: 800;
  color: var(--text-color);
  margin-bottom: 20px;
}
</style>
