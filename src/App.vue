<script setup lang="ts">
import { computed, ref } from 'vue'
import {
  ArrowLeft,
  ArrowUpLeft,
  ShieldCheck,
  Truck,
  Headphones,
  Award,
  Check,
  Wrench,
  ChevronLeft,
  Instagram,
  Send,
  Phone,
  MapPin,
  Menu,
  X,
  Search,
  ShoppingBag,
  ChevronDown,
  Star,
} from 'lucide-vue-next'

interface Product {
  id: number
  name: string
  category: string
  price: number
  oldPrice?: number
  badge?: string
  image: string
  rating: number
}

const menuOpen = ref(false)
const searchOpen = ref(false)
const searchQuery = ref('')
const selectedCategory = ref('همه')

const navLinks = [
  { label: 'صفحه اصلی', href: '#home' },
  { label: 'دسته‌بندی قطعات', href: '#categories' },
  { label: 'محصولات', href: '#products' },
  { label: 'درباره ما', href: '#about' },
]

const categories = [
  { title: 'موتور و متعلقات', subtitle: 'قطعات حیاتی قلب خودرو', icon: '⚙️', count: '۲۴۰+ محصول' },
  { title: 'سیستم ترمز', subtitle: 'کنترل مطمئن در هر مسیر', icon: '◉', count: '۱۸۰+ محصول' },
  { title: 'جلوبندی و تعلیق', subtitle: 'رانندگی نرم و پایدار', icon: '⌁', count: '۱۵۰+ محصول' },
  { title: 'برقی و الکترونیکی', subtitle: 'دقت در تمام جزئیات', icon: 'ϟ', count: '۳۲۰+ محصول' },
]

const products: Product[] = [
  {
    id: 1,
    name: 'دیسک و صفحه کلاچ پریمیوم',
    category: 'سیستم انتقال قدرت',
    price: 2850000,
    oldPrice: 3200000,
    badge: 'پرفروش',
    image:
      'https://images.unsplash.com/photo-1486262715619-67b85e0b08d3?auto=format&fit=crop&w=900&q=85',
    rating: 4.9,
  },
  {
    id: 2,
    name: 'فیلتر هوای اسپرت خودرو',
    category: 'فیلتر و مصرفی',
    price: 890000,
    badge: 'جدید',
    image:
      'https://images.unsplash.com/photo-1619642751034-765dfdf7c58e?auto=format&fit=crop&w=900&q=85',
    rating: 4.8,
  },
  {
    id: 3,
    name: 'لنت ترمز سرامیکی جلو',
    category: 'سیستم ترمز',
    price: 1640000,
    oldPrice: 1890000,
    image:
      'https://images.unsplash.com/photo-1600705722908-bab6e8d2f3c1?auto=format&fit=crop&w=900&q=85',
    rating: 5.0,
  },
  {
    id: 4,
    name: 'کمک فنر اسپرت گازی',
    category: 'جلوبندی و تعلیق',
    price: 4200000,
    image:
      'https://images.unsplash.com/photo-1487754180451-c456f719a1fc?auto=format&fit=crop&w=900&q=85',
    rating: 4.7,
  },
]

const filters = ['همه', ...new Set(products.map((product) => product.category))]
const filteredProducts = computed(() => {
  const query = searchQuery.value.trim()
  return products.filter((product) => {
    const matchesCategory =
      selectedCategory.value === 'همه' || product.category === selectedCategory.value
    const matchesQuery = !query || `${product.name} ${product.category}`.includes(query)
    return matchesCategory && matchesQuery
  })
})

const formatPrice = (price: number) => new Intl.NumberFormat('fa-IR').format(price) + ' تومان'
</script>

<template>
  <header class="site-header">
    <div class="container header-inner">
      <a href="#home" class="brand" aria-label="یدک پلاس، صفحه اصلی">
        <span class="brand-mark">Y<span>+</span></span>
        <span class="brand-copy"
          ><strong>یدک پلاس</strong><small>قطعات مطمئن، مسیر مطمئن</small></span
        >
      </a>

      <nav class="desktop-nav" aria-label="منوی اصلی">
        <a v-for="link in navLinks" :key="link.href" :href="link.href">{{ link.label }}</a>
        <a href="#contact" class="nav-more">ارتباط با ما <ChevronDown :size="14" /></a>
      </nav>

      <div class="header-actions">
        <button
          class="icon-button search-toggle"
          aria-label="جستجو"
          @click="searchOpen = !searchOpen"
        >
          <Search :size="19" />
        </button>
        <a href="#products" class="cart-button"
          ><ShoppingBag :size="17" /><span>مشاهده قطعات</span></a
        >
        <button
          class="icon-button mobile-toggle"
          aria-label="باز کردن منو"
          @click="menuOpen = !menuOpen"
        >
          <X v-if="menuOpen" :size="22" /><Menu v-else :size="22" />
        </button>
      </div>
    </div>

    <div v-if="searchOpen" class="search-panel container">
      <Search :size="18" />
      <input
        v-model="searchQuery"
        autofocus
        placeholder="نام قطعه یا دسته‌بندی را جستجو کنید..."
        @keyup.enter="searchOpen = false"
      />
      <button
        @click="
          searchQuery = '',
          searchOpen = false
        "
      >
        بستن
      </button>
    </div>

    <nav v-if="menuOpen" class="mobile-nav">
      <a v-for="link in navLinks" :key="link.href" :href="link.href" @click="menuOpen = false">{{
        link.label
      }}</a>
      <a href="#contact" @click="menuOpen = false">ارتباط با ما</a>
    </nav>
  </header>

  <main>
    <section id="home" class="hero-section">
      <div class="hero-grid"></div>
      <div class="container hero-content">
        <div class="hero-copy">
          <div class="eyebrow"><span class="eyebrow-dot"></span> همراه مطمئن خودروی شما</div>
          <h1>هر قطعه‌ای که<br />خودروت <span>لازم داره.</span></h1>
          <p>از قطعات مصرفی تا تخصصی؛ با خیال راحت انتخاب کن، با اطمینان برون.</p>
          <div class="hero-actions">
            <a class="button button-primary" href="#products"
              >مشاهده محصولات <ArrowLeft :size="18"
            /></a>
            <a class="button button-ghost" href="#categories"
              >دسته‌بندی قطعات <ArrowUpLeft :size="17"
            /></a>
          </div>
          <div class="hero-proof">
            <div class="proof-avatars"><span>ی</span><span>پ</span><span>+</span></div>
            <div>
              <strong>انتخاب مطمئن راننده‌ها</strong><small>کیفیت، اصالت و پشتیبانی کنار هم</small>
            </div>
          </div>
        </div>
        <div class="hero-visual">
          <div class="hero-image-frame">
            <img
              src="https://images.unsplash.com/photo-1492144534655-ae79c964c9d7?auto=format&fit=crop&w=1500&q=90"
              alt="خودروی اسپرت مشکی"
            />
            <div class="image-shade"></div>
            <div class="hero-image-caption">
              <span>01 / PERFORMANCE</span><strong>برای عملکردی<br />فراتر از انتظار.</strong>
            </div>
          </div>
          <div class="floating-card">
            <span class="floating-icon"><ShieldCheck :size="21" /></span>
            <div><strong>ضمانت اصالت کالا</strong><small>خرید با اطمینان کامل</small></div>
            <Check :size="17" class="floating-check" />
          </div>
          <span class="hero-vertical-text">DRIVE WITH CONFIDENCE — YADAK PLUS</span>
          <span class="hero-orbit"></span>
        </div>
      </div>
      <div class="hero-bottom container">
        <span>01</span>
        <div class="hero-line"><i></i></div>
        <span>03</span><span class="scroll-label">اسکرول کنید <ChevronLeft :size="14" /></span>
      </div>
    </section>

    <section class="trust-strip">
      <div class="container trust-grid">
        <div class="trust-item">
          <ShieldCheck />
          <div><strong>ضمانت اصالت کالا</strong><span>با خیال آسوده خرید کن</span></div>
        </div>
        <div class="trust-item">
          <Truck />
          <div><strong>ارسال سریع و مطمئن</strong><span>در کوتاه‌ترین زمان</span></div>
        </div>
        <div class="trust-item">
          <Headphones />
          <div><strong>مشاوره تخصصی</strong><span>قبل و بعد از خرید</span></div>
        </div>
        <div class="trust-item">
          <Award />
          <div><strong>تنوع برند و قطعه</strong><span>برای خودروهای مختلف</span></div>
        </div>
      </div>
    </section>

    <section id="categories" class="section categories-section">
      <div class="container">
        <div class="section-heading">
          <div>
            <span class="section-kicker">WHAT DO YOU NEED?</span>
            <h2>دسته‌بندی <span>قطعات</span></h2>
            <p>سریع‌تر به قطعه مورد نظرت برس.</p>
          </div>
          <a href="#products" class="text-link">همه محصولات <ArrowLeft :size="17" /></a>
        </div>
        <div class="category-grid">
          <a
            v-for="(category, index) in categories"
            :key="category.title"
            href="#products"
            class="category-card"
          >
            <div class="category-top">
              <span class="category-number">0{{ index + 1 }}</span
              ><span class="category-icon">{{ category.icon }}</span>
            </div>
            <div>
              <h3>{{ category.title }}</h3>
              <p>{{ category.subtitle }}</p>
            </div>
            <div class="category-bottom">
              <span>{{ category.count }}</span
              ><span class="category-link"><ArrowUpLeft :size="17" /></span>
            </div>
          </a>
        </div>
      </div>
    </section>

    <section id="products" class="section products-section">
      <div class="container">
        <div class="section-heading">
          <div>
            <span class="section-kicker">OUR SELECTION</span>
            <h2>انتخاب‌های <span>محبوب</span></h2>
            <p>قطعاتی که کیفیت‌شون امتحانش رو پس داده.</p>
          </div>
          <a href="#contact" class="text-link">راهنمای خرید <ArrowLeft :size="17" /></a>
        </div>
        <div class="filter-row">
          <button
            v-for="filter in filters"
            :key="filter"
            :class="{ active: selectedCategory === filter }"
            @click="selectedCategory = filter"
          >
            {{ filter }}
          </button>
        </div>
        <div class="product-grid">
          <article v-for="product in filteredProducts" :key="product.id" class="product-card">
            <div class="product-image">
              <img :src="product.image" :alt="product.name" loading="lazy" />
              <span v-if="product.badge" class="product-badge">{{ product.badge }}</span>
              <button class="product-arrow" :aria-label="`مشاهده ${product.name}`">
                <ArrowUpLeft :size="18" />
              </button>
            </div>
            <div class="product-info">
              <div class="product-meta">
                <span>{{ product.category }}</span
                ><span class="rating"
                  ><Star :size="13" fill="currentColor" /> {{ product.rating }}</span
                >
              </div>
              <h3>{{ product.name }}</h3>
              <div class="product-bottom">
                <div class="price-wrap">
                  <del v-if="product.oldPrice">{{ formatPrice(product.oldPrice) }}</del
                  ><strong>{{ formatPrice(product.price) }}</strong>
                </div>
                <button class="add-button" aria-label="افزودن به سبد خرید">+</button>
              </div>
            </div>
          </article>
          <div v-if="filteredProducts.length === 0" class="empty-products">
            محصولی با این عبارت پیدا نشد.
          </div>
        </div>
        <div class="products-note">
          <span>+</span> موجودی و قیمت‌ها نمونه هستند و برای اتصال به فروشگاه واقعی آماده‌سازی
          شده‌اند.
        </div>
      </div>
    </section>

    <section class="promo-section">
      <div class="container promo-inner">
        <div class="promo-copy">
          <span class="section-kicker">BUILT FOR THE ROAD</span>
          <h2>قطعه درست،<br /><span>عملکرد بهتر.</span></h2>
          <p>انتخاب قطعه مناسب فقط یک خرید نیست؛ سرمایه‌گذاری روی ایمنی و عمر خودروی توئه.</p>
          <a href="#contact" class="button button-primary"
            >برای انتخاب قطعه کمک بگیر <ArrowLeft :size="18"
          /></a>
        </div>
        <div class="promo-visual">
          <img
            src="https://images.unsplash.com/photo-1486262715619-67b85e0b08d3?auto=format&fit=crop&w=1200&q=85"
            alt="جزئیات فنی قطعات خودرو"
            loading="lazy"
          /><span class="promo-stamp">QUALITY<br />FIRST</span>
        </div>
      </div>
    </section>

    <section id="about" class="section about-section">
      <div class="container about-grid">
        <div class="about-image">
          <img
            src="https://images.unsplash.com/photo-1487754180451-c456f719a1fc?auto=format&fit=crop&w=1200&q=85"
            alt="تعمیر و نگهداری خودرو"
            loading="lazy"
          />
          <div class="about-image-label">
            <Wrench :size="19" /><span>CARE ABOUT EVERY PART</span>
          </div>
        </div>
        <div class="about-copy">
          <span class="section-kicker">WHY YADAK PLUS?</span>
          <h2>ما فقط قطعه<br />نمی‌فروشیم؛ <span>اعتماد می‌سازیم.</span></h2>
          <p>
            پیدا کردن قطعه مناسب نباید سخت و زمان‌بر باشه. اینجا تلاش می‌کنیم با تنوع کالا، اطلاعات
            شفاف و راهنمایی درست، تجربه خرید مطمئنی برات بسازیم.
          </p>
          <ul>
            <li><Check /> انتخاب قطعه متناسب با مدل خودرو</li>
            <li><Check /> همراهی و مشاوره پیش از خرید</li>
            <li><Check /> توجه به کیفیت و اصالت محصولات</li>
          </ul>
          <a href="#contact" class="text-link">بیشتر با ما آشنا شو <ArrowLeft :size="17" /></a>
        </div>
      </div>
    </section>

    <section class="stats-section">
      <div class="container stats-grid">
        <div><strong>+۱۰۰۰</strong><span>قطعه و محصول متنوع</span></div>
        <div><strong>+۲۰</strong><span>برند معتبر</span></div>
        <div><strong>۷ روز</strong><span>پشتیبانی در هفته</span></div>
        <div><strong>۱۰۰٪</strong><span>تمرکز روی رضایت مشتری</span></div>
      </div>
    </section>

    <section id="contact" class="contact-section">
      <div class="container contact-inner">
        <div>
          <span class="section-kicker">LET'S TALK PARTS</span>
          <h2>دنبال قطعه‌ای؟<br /><span>از ما بپرس.</span></h2>
          <p>مدل خودرو و قطعه مورد نظرت رو بگو تا برای انتخاب درست راهنماییت کنیم.</p>
        </div>
        <div class="contact-card">
          <div class="contact-card-head">
            <span class="contact-icon"><Phone /></span>
            <div><strong>مشاوره و ثبت سفارش</strong><small>پاسخگوی سوالات شما هستیم</small></div>
          </div>
          <a class="contact-phone" href="tel:02100000000">۰۲۱-۰۰۰۰-۰۰۰۰</a
          ><span class="contact-placeholder"
            >شماره تماس نمونه است؛ با اطلاعات فروشگاه جایگزین شود.</span
          >
          <div class="contact-links">
            <a href="https://instagram.com" aria-label="اینستاگرام"><Instagram /></a
            ><a href="https://t.me" aria-label="تلگرام"><Send /></a
            ><a href="#contact" aria-label="آدرس"><MapPin /></a>
          </div>
        </div>
      </div>
    </section>
  </main>

  <footer class="site-footer">
    <div class="container footer-main">
      <div class="footer-brand-col">
        <a href="#home" class="brand"
          ><span class="brand-mark">Y<span>+</span></span
          ><span class="brand-copy"
            ><strong>یدک پلاس</strong><small>قطعات مطمئن، مسیر مطمئن</small></span
          ></a
        >
        <p>
          انتخابی مطمئن برای نگهداری و ارتقای خودرو؛ همراه شما از انتخاب قطعه تا تجربه یک خرید
          آسوده.
        </p>
        <div class="footer-socials">
          <a href="https://instagram.com" aria-label="اینستاگرام"><Instagram /></a
          ><a href="https://t.me" aria-label="تلگرام"><Send /></a
          ><a href="tel:02100000000" aria-label="تماس"><Phone /></a>
        </div>
      </div>
      <div class="footer-links-col">
        <h3>دسترسی سریع</h3>
        <a href="#home">صفحه اصلی</a><a href="#categories">دسته‌بندی قطعات</a
        ><a href="#products">محصولات پرفروش</a><a href="#about">درباره یدک پلاس</a>
      </div>
      <div class="footer-links-col">
        <h3>راهنمای خرید</h3>
        <a href="#contact">مشاوره انتخاب قطعه</a><a href="#contact">ثبت سفارش</a
        ><a href="#contact">پشتیبانی مشتریان</a><a href="#contact">سوالات متداول</a>
      </div>
      <div class="footer-contact-col">
        <h3>در ارتباط باشیم</h3>
        <p><Phone :size="16" /> ۰۲۱-۰۰۰۰-۰۰۰۰</p>
        <p><MapPin :size="16" /> آدرس فروشگاه را اینجا وارد کنید</p>
        <a class="footer-contact-cta" href="#contact"
          >ارتباط با کارشناسان <ArrowLeft :size="16"
        /></a>
      </div>
    </div>
    <div class="container footer-bottom">
      <span>© تمامی حقوق برای یدک پلاس محفوظ است.</span><span>ساخته‌شده برای رانندگی مطمئن‌تر</span
      ><a href="#home" class="back-top">بازگشت به بالا ↑</a>
    </div>
  </footer>
</template>

<style>
@import 'tailwindcss';
:root {
  font-family: 'Vazirmatn', sans-serif;
  color: #f4f2ef;
  background: #101010;
  font-synthesis: none;
  text-rendering: optimizeLegibility;
  font-optical-sizing: auto;
  font-weight: 400;
  --orange: #ff6b24;
  --muted: #92908d;
  --panel: #181818;
  --line: rgba(255, 255, 255, 0.09);
}
* {
  box-sizing: border-box;
}
html {
  scroll-behavior: smooth;
  scroll-padding-top: 90px;
}
body {
  margin: 0;
  min-width: 320px;
  background: #101010;
}
a {
  color: inherit;
  text-decoration: none;
}
button,
input {
  font: inherit;
}
button {
  cursor: pointer;
}
.container {
  width: min(1160px, calc(100% - 48px));
  margin-inline: auto;
}
.site-header {
  height: 82px;
  position: sticky;
  top: 0;
  z-index: 50;
  background: rgba(16, 16, 16, 0.91);
  backdrop-filter: blur(18px);
  border-bottom: 1px solid var(--line);
}
.header-inner {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.brand {
  display: flex;
  align-items: center;
  gap: 10px;
  min-width: 180px;
}
.brand-mark {
  width: 43px;
  height: 43px;
  border: 1px solid rgba(255, 107, 36, 0.55);
  display: grid;
  place-items: center;
  font-size: 18px;
  font-weight: 900;
  color: white;
  transform: skew(-7deg);
  background: linear-gradient(145deg, #24201d, #141414);
}
.brand-mark span {
  color: var(--orange);
}
.brand-copy {
  display: flex;
  flex-direction: column;
  line-height: 1.5;
}
.brand-copy strong {
  font-size: 17px;
  font-weight: 800;
}
.brand-copy small {
  font-size: 9px;
  color: #92908d;
}
.desktop-nav {
  display: flex;
  align-items: center;
  gap: 31px;
}
.desktop-nav a {
  font-size: 12px;
  color: #b7b4b0;
  transition: 0.2s;
}
.desktop-nav a:hover {
  color: var(--orange);
}
.nav-more {
  display: flex;
  align-items: center;
  gap: 5px;
}
.header-actions {
  display: flex;
  align-items: center;
  gap: 12px;
}
.icon-button {
  width: 39px;
  height: 39px;
  border: 1px solid var(--line);
  background: transparent;
  color: white;
  display: grid;
  place-items: center;
}
.cart-button {
  height: 40px;
  padding: 0 15px;
  display: flex;
  align-items: center;
  gap: 9px;
  background: var(--orange);
  color: #15100d;
  font-size: 11px;
  font-weight: 800;
}
.mobile-toggle {
  display: none;
}
.search-panel {
  position: absolute;
  top: 82px;
  left: 0;
  right: 0;
  height: 64px;
  background: #1b1b1b;
  border: 1px solid var(--line);
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 0 18px;
}
.search-panel input {
  flex: 1;
  background: transparent;
  border: 0;
  outline: 0;
  color: white;
}
.search-panel button {
  border: 0;
  background: transparent;
  color: var(--orange);
}
.mobile-nav {
  display: none;
}
.hero-section {
  position: relative;
  overflow: hidden;
  min-height: 650px;
  background: radial-gradient(ellipse at 74% 45%, rgba(255, 107, 36, 0.13), transparent 38%), #111;
}
.hero-grid {
  position: absolute;
  inset: 0;
  opacity: 0.17;
  background-image:
    linear-gradient(rgba(255, 255, 255, 0.05) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255, 255, 255, 0.05) 1px, transparent 1px);
  background-size: 65px 65px;
  mask-image: linear-gradient(90deg, transparent, #000 55%, #000);
}
.hero-content {
  position: relative;
  display: grid;
  grid-template-columns: 0.85fr 1.15fr;
  align-items: center;
  gap: 55px;
  padding-top: 62px;
  padding-bottom: 65px;
  min-height: 580px;
}
.hero-copy {
  padding-right: 25px;
}
.eyebrow,
.section-kicker {
  font-size: 10px;
  letter-spacing: 1.6px;
  color: var(--orange);
  font-weight: 700;
}
.eyebrow {
  display: flex;
  align-items: center;
  gap: 9px;
  margin-bottom: 22px;
  letter-spacing: 0;
}
.eyebrow-dot {
  width: 7px;
  height: 7px;
  border-radius: 50%;
  background: var(--orange);
  box-shadow: 0 0 13px var(--orange);
}
h1,
h2,
h3,
p {
  margin-top: 0;
}
.hero-copy h1 {
  font-size: clamp(42px, 5.4vw, 70px);
  line-height: 1.35;
  letter-spacing: -2.5px;
  margin: 0 0 15px;
  font-weight: 900;
}
.hero-copy h1 span,
.section-heading h2 span,
.promo-copy h2 span,
.about-copy h2 span,
.contact-inner h2 span {
  color: var(--orange);
}
.hero-copy > p {
  font-size: 14px;
  line-height: 2.2;
  color: #aaa7a3;
  max-width: 410px;
  margin-bottom: 28px;
}
.hero-actions {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
}
.button {
  min-height: 48px;
  padding: 0 19px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 15px;
  font-size: 11px;
  font-weight: 800;
  transition: 0.25s;
}
.button-primary {
  background: var(--orange);
  color: #17110e;
}
.button-primary:hover {
  background: #ff854b;
  transform: translateY(-3px);
}
.button-ghost {
  border: 1px solid #444;
  color: #eee;
}
.button-ghost:hover {
  border-color: var(--orange);
  color: var(--orange);
}
.hero-proof {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-top: 50px;
}
.proof-avatars {
  display: flex;
  direction: ltr;
}
.proof-avatars span {
  width: 29px;
  height: 29px;
  border-radius: 50%;
  display: grid;
  place-items: center;
  background: #30251f;
  border: 2px solid #111;
  color: var(--orange);
  font-size: 10px;
  font-weight: 800;
  margin-left: -7px;
}
.proof-avatars span:nth-child(2) {
  background: #4a3024;
  color: #fff;
}
.proof-avatars span:nth-child(3) {
  background: var(--orange);
  color: #17110e;
}
.hero-proof div:last-child {
  display: flex;
  flex-direction: column;
  gap: 3px;
}
.hero-proof strong {
  font-size: 10px;
}
.hero-proof small {
  font-size: 9px;
  color: var(--muted);
}
.hero-visual {
  position: relative;
  min-width: 0;
}
.hero-image-frame {
  height: 425px;
  position: relative;
  overflow: hidden;
  border: 1px solid rgba(255, 255, 255, 0.15);
  clip-path: polygon(0 0, 94% 0, 100% 8%, 100% 100%, 6% 100%, 0 92%);
}
.hero-image-frame img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
  filter: brightness(0.72) saturate(0.78);
}
.image-shade {
  position: absolute;
  inset: 0;
  background:
    linear-gradient(0deg, rgba(10, 10, 10, 0.88), transparent 65%),
    linear-gradient(90deg, rgba(255, 107, 36, 0.13), transparent 55%);
}
.hero-image-caption {
  position: absolute;
  bottom: 30px;
  right: 32px;
  display: flex;
  flex-direction: column;
  gap: 9px;
}
.hero-image-caption span {
  font-size: 9px;
  letter-spacing: 2px;
  color: var(--orange);
}
.hero-image-caption strong {
  font-size: 27px;
  line-height: 1.55;
}
.floating-card {
  position: absolute;
  right: -30px;
  bottom: 38px;
  background: #222;
  padding: 14px 17px;
  display: flex;
  align-items: center;
  gap: 11px;
  box-shadow: 0 12px 45px #0008;
  border: 1px solid #ffffff18;
}
.floating-icon {
  width: 37px;
  height: 37px;
  background: #39251b;
  color: var(--orange);
  display: grid;
  place-items: center;
}
.floating-card div {
  display: flex;
  flex-direction: column;
  gap: 4px;
}
.floating-card strong {
  font-size: 10px;
}
.floating-card small {
  font-size: 9px;
  color: var(--muted);
}
.floating-check {
  color: #6dc18a;
}
.hero-vertical-text {
  position: absolute;
  left: -31px;
  top: 50%;
  writing-mode: vertical-rl;
  transform: rotate(180deg);
  font-size: 8px;
  letter-spacing: 2px;
  color: #6b6865;
}
.hero-orbit {
  position: absolute;
  width: 80px;
  height: 80px;
  border: 1px solid #ff6b2440;
  border-radius: 50%;
  top: -25px;
  right: -25px;
}
.hero-bottom {
  position: relative;
  display: flex;
  align-items: center;
  gap: 12px;
  padding-bottom: 20px;
  color: #777;
  font-size: 9px;
}
.hero-line {
  width: 95px;
  height: 2px;
  background: #333;
}
.hero-line i {
  display: block;
  width: 35%;
  height: 100%;
  background: var(--orange);
}
.scroll-label {
  margin-right: auto;
  display: flex;
  align-items: center;
  gap: 5px;
}
.trust-strip {
  background: #191919;
  border-block: 1px solid var(--line);
}
.trust-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  padding: 25px 0;
}
.trust-item {
  display: flex;
  align-items: center;
  gap: 13px;
  padding: 0 22px;
  border-left: 1px solid var(--line);
}
.trust-item:first-child {
  padding-right: 0;
}
.trust-item:last-child {
  border: 0;
}
.trust-item > svg {
  width: 24px;
  height: 24px;
  color: var(--orange);
  stroke-width: 1.5;
}
.trust-item div {
  display: flex;
  flex-direction: column;
  gap: 4px;
}
.trust-item strong {
  font-size: 11px;
}
.trust-item span {
  font-size: 9px;
  color: var(--muted);
}
.section {
  padding: 105px 0;
}
.section-heading {
  display: flex;
  align-items: end;
  justify-content: space-between;
  margin-bottom: 35px;
}
.section-heading h2,
.promo-copy h2,
.about-copy h2,
.contact-inner h2 {
  font-size: 35px;
  line-height: 1.5;
  margin: 9px 0 5px;
  font-weight: 900;
}
.section-heading p,
.promo-copy p,
.about-copy > p,
.contact-inner p {
  font-size: 12px;
  color: var(--muted);
  line-height: 2.1;
  margin: 0;
}
.text-link {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  color: var(--orange);
  font-size: 11px;
  font-weight: 700;
  padding-bottom: 7px;
}
.category-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 13px;
}
.category-card {
  background: #191919;
  border: 1px solid var(--line);
  padding: 21px 20px 17px;
  min-height: 205px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  transition: 0.25s;
}
.category-card:hover {
  border-color: #ff6b247a;
  transform: translateY(-5px);
  background: #1e1c1a;
}
.category-top,
.category-bottom {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.category-number {
  font-size: 10px;
  color: #777;
  letter-spacing: 1px;
}
.category-icon {
  font-size: 29px;
  color: var(--orange);
}
.category-card h3 {
  font-size: 15px;
  margin: 0 0 5px;
}
.category-card p {
  font-size: 10px;
  color: var(--muted);
  margin: 0;
}
.category-bottom {
  border-top: 1px solid var(--line);
  padding-top: 13px;
  color: #8b8884;
  font-size: 9px;
}
.category-link {
  width: 28px;
  height: 28px;
  background: #2b211b;
  color: var(--orange);
  display: grid;
  place-items: center;
}
.products-section {
  background: #171717;
}
.filter-row {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
  margin-bottom: 22px;
}
.filter-row button {
  background: #202020;
  color: #aaa;
  border: 1px solid var(--line);
  padding: 8px 15px;
  font-size: 10px;
}
.filter-row button.active,
.filter-row button:hover {
  background: var(--orange);
  border-color: var(--orange);
  color: #17110e;
}
.product-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 14px;
}
.product-card {
  background: #202020;
  border: 1px solid var(--line);
  min-width: 0;
  transition: 0.25s;
}
.product-card:hover {
  transform: translateY(-5px);
  border-color: #ff6b2466;
}
.product-image {
  height: 190px;
  position: relative;
  overflow: hidden;
  background: #292929;
}
.product-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter: brightness(0.8) saturate(0.75);
  transition: 0.5s;
}
.product-card:hover .product-image img {
  transform: scale(1.06);
  filter: brightness(0.95) saturate(0.9);
}
.product-badge {
  position: absolute;
  top: 12px;
  right: 12px;
  background: var(--orange);
  color: #1a110d;
  font-size: 9px;
  padding: 5px 9px;
  font-weight: 800;
}
.product-arrow {
  position: absolute;
  left: 11px;
  bottom: 11px;
  width: 32px;
  height: 32px;
  border: 1px solid #ffffff60;
  background: #141414b8;
  color: white;
  display: grid;
  place-items: center;
}
.product-info {
  padding: 14px;
}
.product-meta {
  display: flex;
  align-items: center;
  justify-content: space-between;
  color: #92908d;
  font-size: 9px;
}
.rating {
  display: flex;
  align-items: center;
  gap: 4px;
  color: #eab65b;
}
.product-info h3 {
  font-size: 12px;
  margin: 12px 0 18px;
  min-height: 20px;
}
.product-bottom {
  display: flex;
  justify-content: space-between;
  align-items: end;
  border-top: 1px solid var(--line);
  padding-top: 12px;
}
.price-wrap {
  display: flex;
  flex-direction: column;
  gap: 3px;
}
.price-wrap del {
  font-size: 9px;
  color: #777;
}
.price-wrap strong {
  font-size: 11px;
  color: #f2efeb;
}
.add-button {
  width: 30px;
  height: 30px;
  border: 0;
  background: var(--orange);
  font-size: 21px;
  color: #15100d;
}
.products-note {
  font-size: 9px;
  color: #777;
  margin-top: 18px;
  display: flex;
  align-items: center;
  gap: 8px;
}
.products-note span {
  font-size: 16px;
  color: var(--orange);
}
.promo-section {
  background: linear-gradient(110deg, #1b1b1b, #121212);
  padding: 85px 0;
  overflow: hidden;
}
.promo-inner {
  display: grid;
  grid-template-columns: 0.85fr 1.15fr;
  align-items: center;
  gap: 65px;
}
.promo-copy h2 {
  font-size: 43px;
  margin: 12px 0;
}
.promo-copy p {
  max-width: 370px;
  margin-bottom: 25px;
}
.promo-visual {
  height: 320px;
  position: relative;
}
.promo-visual img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter: brightness(0.65) saturate(0.7);
  clip-path: polygon(7% 0, 100% 0, 100% 100%, 0 100%, 0 13%);
}
.promo-stamp {
  position: absolute;
  left: 24px;
  bottom: 22px;
  border: 1px solid var(--orange);
  color: var(--orange);
  padding: 12px 17px;
  font-size: 12px;
  line-height: 1.5;
  font-weight: 900;
  letter-spacing: 2px;
  transform: rotate(-8deg);
}
.about-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;
  gap: 75px;
}
.about-image {
  height: 410px;
  position: relative;
}
.about-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter: brightness(0.72) saturate(0.7);
}
.about-image:after {
  content: '';
  position: absolute;
  inset: 13px -13px -13px 13px;
  border: 1px solid #ff6b2470;
  z-index: -1;
}
.about-image-label {
  position: absolute;
  right: 0;
  bottom: 0;
  background: var(--orange);
  color: #17110d;
  padding: 12px 16px;
  display: flex;
  align-items: center;
  gap: 9px;
  font-size: 9px;
  font-weight: 900;
  letter-spacing: 1px;
}
.about-copy h2 {
  font-size: 36px;
  margin: 10px 0;
}
.about-copy > p {
  margin-bottom: 20px;
}
.about-copy ul {
  list-style: none;
  padding: 0;
  margin: 0 0 22px;
  display: grid;
  gap: 13px;
}
.about-copy li {
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 11px;
  color: #d0cecb;
}
.about-copy li svg {
  width: 17px;
  height: 17px;
  color: var(--orange);
}
.stats-section {
  background: #1a1a1a;
  border-block: 1px solid var(--line);
}
.stats-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  padding: 31px 0;
}
.stats-grid > div {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  border-left: 1px solid var(--line);
}
.stats-grid > div:last-child {
  border: 0;
}
.stats-grid strong {
  font-size: 26px;
  color: var(--orange);
  font-weight: 900;
}
.stats-grid span {
  font-size: 10px;
  color: #aaa;
}
.contact-section {
  padding: 90px 0;
  background: radial-gradient(ellipse at 80% 50%, #ff6b2415, transparent 35%), #111;
}
.contact-inner {
  display: grid;
  grid-template-columns: 1fr 390px;
  align-items: center;
  gap: 70px;
}
.contact-inner h2 {
  font-size: 42px;
  margin: 12px 0;
}
.contact-inner p {
  max-width: 400px;
}
.contact-card {
  background: #1d1d1d;
  border: 1px solid var(--line);
  padding: 25px;
}
.contact-card-head {
  display: flex;
  align-items: center;
  gap: 12px;
}
.contact-icon {
  width: 43px;
  height: 43px;
  background: #39251b;
  color: var(--orange);
  display: grid;
  place-items: center;
}
.contact-card-head div {
  display: flex;
  flex-direction: column;
  gap: 4px;
}
.contact-card-head strong {
  font-size: 12px;
}
.contact-card-head small {
  font-size: 9px;
  color: var(--muted);
}
.contact-phone {
  display: block;
  font-size: 25px;
  font-weight: 900;
  direction: ltr;
  text-align: right;
  margin: 22px 0 3px;
}
.contact-placeholder {
  font-size: 9px;
  color: #777;
}
.contact-links {
  display: flex;
  gap: 8px;
  border-top: 1px solid var(--line);
  padding-top: 17px;
  margin-top: 18px;
}
.contact-links a {
  width: 33px;
  height: 33px;
  border: 1px solid var(--line);
  display: grid;
  place-items: center;
  color: #aaa;
}
.contact-links a:hover {
  color: var(--orange);
  border-color: var(--orange);
}
.contact-links svg {
  width: 15px;
}
.site-footer {
  background: #191919;
  border-top: 1px solid var(--line);
}
.footer-inner {
  min-height: 90px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.footer-inner > span {
  font-size: 10px;
  color: #777;
}
.back-top {
  font-size: 10px;
  color: var(--orange);
}
@media (max-width: 900px) {
  .desktop-nav {
    gap: 16px;
  }
  .desktop-nav a {
    font-size: 10px;
  }
  .hero-content {
    gap: 25px;
  }
  .hero-copy {
    padding-right: 0;
  }
  .hero-image-frame {
    height: 360px;
  }
  .floating-card {
    right: -10px;
  }
  .trust-item {
    padding: 0 12px;
  }
  .trust-item strong {
    font-size: 10px;
  }
  .category-grid,
  .product-grid {
    grid-template-columns: repeat(2, 1fr);
  }
  .about-grid {
    gap: 40px;
  }
  .promo-inner {
    gap: 35px;
  }
}
@media (max-width: 640px) {
  .container {
    width: min(100% - 34px, 500px);
  }
  .site-header {
    height: 70px;
  }
  .brand {
    min-width: 0;
  }
  .brand-mark {
    width: 38px;
    height: 38px;
  }
  .brand-copy strong {
    font-size: 15px;
  }
  .desktop-nav {
    display: none;
  }
  .header-actions {
    gap: 7px;
  }
  .search-toggle {
    width: 35px;
    height: 35px;
  }
  .cart-button {
    height: 36px;
    padding: 0 10px;
    font-size: 9px;
  }
  .mobile-toggle {
    display: grid;
    width: 35px;
    height: 35px;
  }
  .mobile-nav {
    display: flex;
    position: absolute;
    top: 70px;
    right: 0;
    left: 0;
    flex-direction: column;
    background: #191919;
    border-bottom: 1px solid var(--line);
    padding: 10px 22px 17px;
    box-shadow: 0 15px 25px #0007;
  }
  .mobile-nav a {
    padding: 12px 0;
    border-bottom: 1px solid var(--line);
    font-size: 12px;
    color: #ddd;
  }
  .mobile-nav a:last-child {
    border: 0;
  }
  .search-panel {
    top: 70px;
    width: 100%;
    height: 58px;
  }
  .search-panel input {
    font-size: 11px;
  }
  .hero-content {
    grid-template-columns: 1fr;
    gap: 35px;
    padding-top: 45px;
    padding-bottom: 45px;
  }
  .hero-copy {
    padding: 0;
  }
  .hero-copy h1 {
    font-size: 48px;
    letter-spacing: -1.5px;
  }
  .hero-copy > p {
    font-size: 12px;
  }
  .hero-proof {
    margin-top: 30px;
  }
  .hero-visual {
    margin: 0 8px 0 18px;
  }
  .hero-image-frame {
    height: 280px;
  }
  .hero-image-caption {
    right: 19px;
    bottom: 20px;
  }
  .hero-image-caption strong {
    font-size: 21px;
  }
  .floating-card {
    right: -13px;
    bottom: 18px;
    padding: 10px;
  }
  .floating-card strong {
    font-size: 9px;
  }
  .floating-card small {
    font-size: 8px;
  }
  .hero-vertical-text {
    left: -20px;
    font-size: 6px;
  }
  .hero-orbit {
    width: 55px;
    height: 55px;
  }
  .hero-bottom {
    padding-bottom: 17px;
  }
  .trust-grid {
    grid-template-columns: repeat(2, 1fr);
    row-gap: 20px;
    padding: 22px 0;
  }
  .trust-item {
    border: 0;
    padding: 0 5px;
  }
  .trust-item:nth-child(odd) {
    border-left: 1px solid var(--line);
  }
  .trust-item > svg {
    width: 21px;
  }
  .trust-item strong {
    font-size: 10px;
  }
  .trust-item span {
    font-size: 8px;
  }
  .section {
    padding: 70px 0;
  }
  .section-heading {
    align-items: start;
    gap: 12px;
  }
  .section-heading h2 {
    font-size: 27px;
  }
  .section-heading p {
    font-size: 10px;
  }
  .section-heading > .text-link {
    font-size: 9px;
    white-space: nowrap;
    margin-top: 27px;
  }
  .category-grid {
    gap: 9px;
  }
  .category-card {
    padding: 15px;
    min-height: 180px;
  }
  .category-card h3 {
    font-size: 12px;
  }
  .category-card p {
    font-size: 9px;
  }
  .category-icon {
    font-size: 24px;
  }
  .category-bottom {
    font-size: 8px;
  }
  .filter-row {
    gap: 6px;
  }
  .filter-row button {
    padding: 7px 10px;
    font-size: 9px;
  }
  .product-grid {
    gap: 9px;
  }
  .product-image {
    height: 135px;
  }
  .product-info {
    padding: 10px;
  }
  .product-info h3 {
    font-size: 10px;
    margin: 10px 0 13px;
  }
  .product-meta {
    font-size: 8px;
  }
  .price-wrap strong {
    font-size: 9px;
  }
  .price-wrap del {
    font-size: 8px;
  }
  .add-button {
    width: 26px;
    height: 26px;
  }
  .products-note {
    font-size: 8px;
    line-height: 1.8;
  }
  .promo-section {
    padding: 65px 0;
  }
  .promo-inner {
    grid-template-columns: 1fr;
    gap: 28px;
  }
  .promo-copy h2 {
    font-size: 36px;
  }
  .promo-copy p {
    font-size: 11px;
  }
  .promo-visual {
    height: 230px;
    margin-left: 8px;
  }
  .about-grid {
    grid-template-columns: 1fr;
    gap: 40px;
  }
  .about-image {
    height: 270px;
    margin-left: 10px;
  }
  .about-copy h2 {
    font-size: 30px;
  }
  .about-copy > p {
    font-size: 11px;
  }
  .stats-grid {
    grid-template-columns: repeat(2, 1fr);
    row-gap: 20px;
  }
  .stats-grid > div:nth-child(2) {
    border: 0;
  }
  .stats-grid strong {
    font-size: 23px;
  }
  .stats-grid span {
    font-size: 9px;
  }
  .contact-section {
    padding: 65px 0;
  }
  .contact-inner {
    grid-template-columns: 1fr;
    gap: 30px;
  }
  .contact-inner h2 {
    font-size: 36px;
  }
  .contact-card {
    padding: 19px;
  }
  .footer-inner {
    padding: 18px 0;
    gap: 14px;
    flex-wrap: wrap;
  }
  .footer-inner > span {
    order: 3;
    width: 100%;
    font-size: 9px;
  }
  .back-top {
    font-size: 9px;
  }
}
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    scroll-behavior: auto !important;
    transition: none !important;
    animation: none !important;
  }
}

/* White + blue theme and consistent Persian RTL layout */
:root {
  direction: rtl;
  color: #172b4d;
  background: #fff;
  --orange: #1769d2;
  --muted: #687991;
  --panel: #f5f8fc;
  --line: rgba(25, 65, 115, 0.12);
}
html,
body,
#app {
  direction: rtl;
  background: #fff;
  color: #172b4d;
}
body {
  background: #fff;
}
.site-header {
  background: rgba(255, 255, 255, 0.94);
  border-bottom: 1px solid var(--line);
}
.brand-mark {
  color: #fff;
  border-color: #1769d2;
  background: linear-gradient(145deg, #287fe7, #1255b2);
}
.brand-mark span,
.desktop-nav a:hover {
  color: #b9dcff;
}
.brand-copy small,
.desktop-nav a {
  color: #687991;
}
.icon-button {
  color: #1769d2;
  background: #f5f9ff;
  border-color: var(--line);
}
.cart-button,
.button-primary,
.filter-row button.active,
.filter-row button:hover,
.add-button {
  background: #1769d2;
  color: #fff;
  border-color: #1769d2;
}
.search-panel,
.mobile-nav {
  background: #fff;
  border-color: var(--line);
  box-shadow: 0 12px 30px rgba(20, 55, 100, 0.08);
}
.search-panel input {
  color: #172b4d;
}
.search-panel button {
  color: #1769d2;
}
.mobile-nav a {
  color: #263d5d;
  border-color: var(--line);
}
.hero-section {
  background:
    radial-gradient(ellipse at 74% 45%, rgba(23, 105, 210, 0.1), transparent 38%), #f5f9ff;
  color: #172b4d;
}
.hero-grid {
  background-image:
    linear-gradient(rgba(23, 105, 210, 0.07) 1px, transparent 1px),
    linear-gradient(90deg, rgba(23, 105, 210, 0.07) 1px, transparent 1px);
}
.hero-copy h1,
.section-heading h2,
.promo-copy h2,
.about-copy h2,
.contact-inner h2 {
  color: #172b4d;
}
.hero-copy > p,
.section-heading p,
.promo-copy p,
.about-copy > p,
.contact-inner p {
  color: #687991;
}
.hero-image-frame {
  border-color: rgba(23, 105, 210, 0.18);
}
.image-shade {
  background:
    linear-gradient(0deg, rgba(8, 28, 55, 0.78), transparent 65%),
    linear-gradient(90deg, rgba(23, 105, 210, 0.12), transparent 55%);
}
.floating-card {
  background: #fff;
  color: #172b4d;
  border-color: var(--line);
  box-shadow: 0 12px 35px rgba(20, 55, 100, 0.16);
}
.floating-icon,
.category-link,
.contact-icon {
  background: #e8f2ff;
  color: #1769d2;
}
.hero-vertical-text,
.hero-bottom {
  color: #71839b;
}
.hero-line {
  background: #d8e3f1;
}
.hero-line i {
  background: #1769d2;
}
.trust-strip,
.stats-section {
  background: #f7faff;
  border-color: var(--line);
}
.trust-item {
  border-color: var(--line);
}
.trust-item strong {
  color: #203957;
}
.trust-item span {
  color: #71839b;
}
.section {
  background: #fff;
}
.category-card,
.product-card,
.contact-card {
  background: #fff;
  border-color: var(--line);
  box-shadow: 0 8px 26px rgba(25, 65, 115, 0.055);
}
.category-card:hover,
.product-card:hover {
  background: #f8fbff;
  border-color: #8bbaf0;
}
.category-number,
.category-bottom,
.product-meta {
  color: #71839b;
}
.category-card p {
  color: #71839b;
}
.category-bottom,
.product-bottom {
  border-color: var(--line);
}
.products-section {
  background: #f5f8fc;
}
.filter-row button {
  background: #fff;
  color: #536781;
  border-color: var(--line);
}
.product-image {
  background: #eaf1f9;
}
.product-image img {
  filter: none;
}
.product-card:hover .product-image img {
  filter: none;
}
.product-badge {
  background: #1769d2;
  color: #fff;
}
.product-arrow {
  background: rgba(15, 48, 88, 0.78);
  color: #fff;
}
.price-wrap strong {
  color: #1769d2;
}
.price-wrap del,
.products-note {
  color: #8796a9;
}
.promo-section {
  background: linear-gradient(110deg, #edf5ff, #f8fbff);
  color: #172b4d;
}
.promo-visual img {
  filter: brightness(0.82) saturate(0.85);
}
.promo-stamp {
  border-color: #fff;
  color: #fff;
}
.about-image:after {
  border-color: #8bbaf0;
}
.about-image-label {
  background: #1769d2;
  color: #fff;
}
.stats-grid > div {
  border-color: var(--line);
}
.stats-grid strong {
  color: #1769d2;
}
.stats-grid span {
  color: #687991;
}
.contact-section {
  background:
    radial-gradient(ellipse at 80% 50%, rgba(23, 105, 210, 0.08), transparent 35%), #f5f9ff;
}
.contact-card {
  background: #fff;
}
.contact-card-head small,
.contact-placeholder {
  color: #71839b;
}
.contact-links {
  border-color: var(--line);
}
.contact-links a {
  border-color: var(--line);
  color: #60748e;
}
.contact-links a:hover {
  color: #1769d2;
  border-color: #1769d2;
}
.site-footer {
  background: #f1f6fc;
  border-color: var(--line);
}
.footer-inner > span {
  color: #71839b;
}
.back-top,
.text-link {
  color: #1769d2;
}
.button-ghost {
  border-color: #c8d7e8;
  color: #31506f;
}
.button-ghost:hover {
  border-color: #1769d2;
  color: #1769d2;
}
.hero-proof small {
  color: #71839b;
}
.proof-avatars span {
  background: #dcecff;
  border-color: #f5f9ff;
  color: #1769d2;
}
.proof-avatars span:nth-child(2) {
  background: #b9d7fb;
  color: #174a88;
}
.proof-avatars span:nth-child(3) {
  background: #1769d2;
  color: #fff;
}
@media (max-width: 640px) {
  .mobile-nav {
    background: #fff;
  }
  .mobile-nav a {
    color: #263d5d;
  }
}

/* Premium polish: rounded surfaces, refined navigation and motion */
:root {
  --blue: #1769d2;
  --blue-deep: #104f9f;
  --ice: #eaf3ff;
  --radius-xl: 24px;
  --radius-lg: 18px;
  --radius-md: 13px;
}
.site-header {
  height: 88px;
  background: rgba(255, 255, 255, 0.86);
  backdrop-filter: blur(22px);
  box-shadow: 0 8px 30px rgba(23, 65, 120, 0.055);
  border-bottom: 1px solid rgba(23, 105, 210, 0.09);
}
.header-inner {
  gap: 24px;
}
.brand {
  min-width: 190px;
}
.brand-mark {
  border-radius: 14px;
  transform: none;
  box-shadow: 0 7px 18px rgba(23, 105, 210, 0.2);
}
.desktop-nav {
  gap: 5px;
  padding: 7px;
  background: #f3f7fd;
  border: 1px solid #e7eef8;
  border-radius: 999px;
}
.desktop-nav a {
  padding: 11px 15px;
  border-radius: 999px;
  color: #526985;
  font-weight: 650;
  transition:
    background 0.25s,
    color 0.25s,
    transform 0.25s;
}
.desktop-nav a:hover,
.desktop-nav a:focus-visible {
  background: #fff;
  color: var(--blue);
  box-shadow: 0 4px 13px rgba(23, 105, 210, 0.1);
  transform: translateY(-1px);
}
.nav-more {
  gap: 7px;
}
.header-actions {
  gap: 10px;
}
.icon-button {
  border-radius: 13px;
  transition: 0.25s;
}
.icon-button:hover {
  background: #eaf3ff;
  transform: translateY(-2px);
}
.cart-button {
  border-radius: 13px;
  box-shadow: 0 8px 18px rgba(23, 105, 210, 0.2);
  transition: 0.25s;
}
.cart-button:hover {
  background: #1058b4;
  transform: translateY(-2px);
  box-shadow: 0 12px 24px rgba(23, 105, 210, 0.25);
}
.search-panel {
  border-radius: 0 0 18px 18px;
  box-shadow: 0 18px 35px rgba(20, 55, 100, 0.12);
}
.mobile-nav {
  border-radius: 0 0 18px 18px;
}
.hero-section {
  background:
    radial-gradient(ellipse at 75% 42%, rgba(23, 105, 210, 0.13), transparent 40%),
    linear-gradient(135deg, #f8fbff, #eef5ff);
}
.hero-image-frame {
  border-radius: 26px;
  clip-path: none;
  box-shadow: 0 24px 55px rgba(15, 48, 88, 0.17);
}
.floating-card {
  border-radius: 16px;
}
.floating-icon {
  border-radius: 11px;
}
.button {
  border-radius: 14px;
  min-height: 52px;
  transition:
    transform 0.28s,
    box-shadow 0.28s,
    background 0.28s;
}
.button-primary {
  box-shadow: 0 10px 22px rgba(23, 105, 210, 0.2);
}
.button-primary:hover {
  box-shadow: 0 15px 28px rgba(23, 105, 210, 0.28);
}
.button-ghost {
  background: rgba(255, 255, 255, 0.7);
}
.category-card,
.product-card,
.contact-card {
  border-radius: var(--radius-lg);
  overflow: hidden;
  box-shadow: 0 10px 30px rgba(25, 65, 115, 0.06);
  transition:
    transform 0.35s cubic-bezier(0.2, 0.8, 0.2, 1),
    box-shadow 0.35s,
    border-color 0.35s;
}
.category-card {
  padding: 24px;
  min-height: 220px;
  background: linear-gradient(150deg, #fff, #f9fbff);
}
.category-card:hover,
.product-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 20px 42px rgba(24, 82, 150, 0.13);
}
.category-link,
.add-button,
.product-arrow {
  border-radius: 10px;
  transition:
    transform 0.25s,
    background 0.25s;
}
.category-card:hover .category-link {
  transform: translateX(-3px);
  background: #dcecff;
}
.product-card {
  background: #fff;
}
.product-image {
  height: 205px;
}
.product-image img {
  transition: transform 0.7s cubic-bezier(0.2, 0.8, 0.2, 1);
}
.product-card:hover .product-image img {
  transform: scale(1.075);
}
.product-badge {
  border-radius: 999px;
  padding: 7px 12px;
  box-shadow: 0 5px 14px rgba(23, 105, 210, 0.2);
}
.product-arrow {
  border-radius: 50%;
  width: 38px;
  height: 38px;
}
.add-button {
  border-radius: 10px;
  box-shadow: 0 6px 14px rgba(23, 105, 210, 0.18);
}
.add-button:hover {
  transform: rotate(90deg) scale(1.06);
}
.filter-row button {
  border-radius: 999px;
  padding: 10px 18px;
  transition: 0.25s;
}
.filter-row button:hover {
  transform: translateY(-2px);
}
.promo-section {
  border-block: 1px solid #e6eef8;
}
.promo-visual {
  border-radius: 24px;
  overflow: hidden;
  box-shadow: 0 22px 45px rgba(20, 55, 100, 0.13);
}
.promo-visual img {
  clip-path: none;
}
.promo-stamp {
  border-radius: 12px;
  backdrop-filter: blur(8px);
  background: rgba(16, 79, 159, 0.65);
}
.about-image {
  border-radius: 24px;
}
.about-image img {
  border-radius: 24px;
}
.about-image:after {
  border-radius: 24px;
}
.about-image-label {
  border-radius: 0 14px 0 0;
}
.stats-grid > div {
  padding: 8px;
}
.contact-card {
  padding: 30px;
  border-radius: 22px;
  box-shadow: 0 18px 45px rgba(23, 65, 120, 0.09);
}
.contact-icon {
  border-radius: 13px;
}
.contact-links a {
  border-radius: 10px;
  transition: 0.25s;
}
.contact-links a:hover {
  transform: translateY(-3px);
  background: #eaf3ff;
}
.site-footer {
  background: linear-gradient(145deg, #f4f8fe, #eaf2fc);
  border-top: 1px solid #dce8f6;
  padding-top: 58px;
}
.footer-main {
  display: grid;
  grid-template-columns: 1.45fr 0.8fr 0.9fr 1.15fr;
  gap: 48px;
  padding-bottom: 44px;
}
.footer-brand-col p {
  max-width: 290px;
  color: #71839b;
  font-size: 12px;
  line-height: 2.2;
  margin: 20px 0;
}
.footer-socials {
  display: flex;
  gap: 9px;
}
.footer-socials a {
  width: 38px;
  height: 38px;
  display: grid;
  place-items: center;
  border: 1px solid #d8e5f4;
  border-radius: 12px;
  color: #1769d2;
  background: rgba(255, 255, 255, 0.75);
  transition: 0.25s;
}
.footer-socials a:hover {
  background: #1769d2;
  color: #fff;
  transform: translateY(-4px);
  box-shadow: 0 8px 18px rgba(23, 105, 210, 0.2);
}
.footer-socials svg {
  width: 17px;
  height: 17px;
}
.footer-links-col,
.footer-contact-col {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 13px;
}
.footer-links-col h3,
.footer-contact-col h3 {
  font-size: 13px;
  color: #172b4d;
  margin: 4px 0 8px;
  font-weight: 850;
}
.footer-links-col a {
  font-size: 11px;
  color: #6b7e98;
  transition:
    color 0.2s,
    transform 0.2s;
}
.footer-links-col a:hover {
  color: #1769d2;
  transform: translateX(-3px);
}
.footer-contact-col p {
  display: flex;
  align-items: center;
  gap: 9px;
  color: #6b7e98;
  font-size: 11px;
  margin: 0;
  line-height: 1.8;
}
.footer-contact-col p svg {
  color: #1769d2;
  flex: none;
}
.footer-contact-cta {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-top: 5px;
  padding: 11px 15px;
  border-radius: 12px;
  background: #1769d2;
  color: #fff;
  font-size: 10px;
  font-weight: 800;
  box-shadow: 0 8px 18px rgba(23, 105, 210, 0.18);
  transition: 0.25s;
}
.footer-contact-cta:hover {
  background: #1058b4;
  transform: translateY(-2px);
}
.footer-bottom {
  min-height: 64px;
  border-top: 1px solid #dce6f2;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  color: #8797ac;
  font-size: 10px;
}
.back-top {
  font-weight: 800;
}
@keyframes softRise {
  from {
    opacity: 0;
    transform: translateY(18px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
.hero-copy,
.hero-visual {
  animation: softRise 0.8s both;
}
.hero-visual {
  animation-delay: 0.12s;
}
.trust-item,
.category-card,
.product-card {
  animation: softRise 0.65s both;
}
.category-card:nth-child(2),
.product-card:nth-child(2) {
  animation-delay: 0.07s;
}
.category-card:nth-child(3),
.product-card:nth-child(3) {
  animation-delay: 0.14s;
}
.category-card:nth-child(4),
.product-card:nth-child(4) {
  animation-delay: 0.21s;
}
a,
button {
  -webkit-tap-highlight-color: transparent;
}
:focus-visible {
  outline: 3px solid rgba(23, 105, 210, 0.35);
  outline-offset: 3px;
}
@media (max-width: 900px) {
  .footer-main {
    grid-template-columns: 1fr 1fr;
    gap: 35px;
  }
  .desktop-nav {
    gap: 2px;
  }
  .desktop-nav a {
    padding: 10px 11px;
  }
}
@media (max-width: 640px) {
  .site-header {
    height: 74px;
  }
  .brand {
    min-width: 0;
  }
  .desktop-nav {
    display: none;
  }
  .mobile-nav {
    top: 74px;
  }
  .search-panel {
    top: 74px;
  }
  .cart-button {
    border-radius: 11px;
  }
  .hero-image-frame {
    border-radius: 19px;
  }
  .category-card {
    border-radius: 15px;
    min-height: 190px;
    padding: 17px;
  }
  .product-card {
    border-radius: 15px;
  }
  .product-image {
    height: 145px;
  }
  .button {
    border-radius: 12px;
  }
  .footer-main {
    grid-template-columns: 1fr 1fr;
    gap: 30px 20px;
    padding-bottom: 30px;
  }
  .footer-brand-col {
    grid-column: 1/-1;
  }
  .footer-brand-col p {
    max-width: 100%;
  }
  .footer-links-col h3,
  .footer-contact-col h3 {
    font-size: 12px;
  }
  .footer-links-col a,
  .footer-contact-col p {
    font-size: 10px;
  }
  .footer-contact-cta {
    font-size: 9px;
    padding: 10px 11px;
  }
  .footer-bottom {
    padding: 16px 0;
    flex-wrap: wrap;
    justify-content: space-between;
  }
  .footer-bottom span:nth-child(2) {
    display: none;
  }
}
@media (prefers-reduced-motion: reduce) {
  .hero-copy,
  .hero-visual,
  .trust-item,
  .category-card,
  .product-card {
    animation: none !important;
  }
}
</style>
