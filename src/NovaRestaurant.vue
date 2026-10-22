<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  language: { type: String, default: 'ar' },
})
const emit = defineEmits(['change-language'])

const isArabic = computed(() => props.language === 'ar')
const activeCategory = ref('mains')
const showBackToTop = ref(false)

// ===== ترجمة كاملة =====
const t = computed(() => ({
  langButton: isArabic.value ? 'FR' : 'AR',
  backToTop: isArabic.value ? 'العودة إلى الأعلى' : 'Retour en haut',
  priceNote: isArabic.value ? 'الأسعار بالدينار التونسي' : 'Prix en Dinar Tunisien',
  vegetarian: isArabic.value ? 'نباتي' : 'Végétarien',
  glutenFree: isArabic.value ? 'خالٍ من الغلوتين' : 'Sans gluten',
  chef: isArabic.value ? 'اختيار الشيف' : 'Choix du Chef',
  lastThree: isArabic.value ? 'متبقي 3 فقط' : 'Plus que 3',
  address: isArabic.value ? 'العنوان' : 'Adresse',
  phone: isArabic.value ? 'الهاتف' : 'Téléphone',
  hours: isArabic.value ? 'ساعات العمل' : 'Horaires',
  followUs: isArabic.value ? 'تابعونا' : 'Suivez-nous',
}))

// ===== بيانات المطعم =====
const restaurantInfo = computed(() => ({
  name: 'NOVA',
  tagline: isArabic.value ? 'مطبخ متوسطي · تونس' : 'Cuisine Méditerranéenne · Tunis',
  established: '2022',
  address: isArabic.value ? '8 شارع الحبيب بورقيبة، تونس' : '8 Avenue Habib Bourguiba, Tunis',
  phone: '+216 71 987 654',
  hours: isArabic.value ? 'الثلاثاء - الأحد: 12:00 - 15:00 / 19:00 - 23:00' : 'Mar - Dim : 12h - 15h / 19h - 23h',
  instagram: '@nova.restaurant.tn',
  rating: '4.8',
  reviews: '89',
}))

const categories = computed(() => [
  { id: 'mains', name: isArabic.value ? 'أطباق رئيسية' : 'Plats' },
  { id: 'starters', name: isArabic.value ? 'مقبلات' : 'Entrées' },
  { id: 'pasta', name: isArabic.value ? 'معكرونة' : 'Pâtes' },
  { id: 'grill', name: isArabic.value ? 'مشاوي' : 'Grillades' },
  { id: 'desserts', name: isArabic.value ? 'حلويات' : 'Desserts' },
  { id: 'drinks', name: isArabic.value ? 'مشروبات' : 'Boissons' },
])

const menuItems = computed(() => [
  // 🥩 رئيسية
  { id: 1, category: 'mains', name: isArabic.value ? 'كسكسي بالعلوش' : 'Couscous d\'Agneau', description: isArabic.value ? 'كسكسي تقليدي، لحم علوش مطهو ببطء، خضار موسمية' : 'Couscous traditionnel, agneau mijoté, légumes', price: 42.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1541518763669-27fef04b14ea?w=500', badge: 'chef', dietary: [] },
  { id: 2, category: 'mains', name: isArabic.value ? 'سمك القاروص المشوي' : 'Loup de Mer Grillé', description: isArabic.value ? 'قاروص كامل، ليمون مشوي، زيت زيتون، أعشاب' : 'Loup entier, citron grillé, huile d\'olive, herbes', price: 55.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1535140728325-a4d3707eee61?w=500', badge: null, dietary: ['GF'] },
  { id: 3, category: 'mains', name: isArabic.value ? 'ريزوتو الكمأة' : 'Risotto Truffe', description: isArabic.value ? 'أرز أربوريو، كمأة سوداء، بارميزان' : 'Riz arborio, truffe noire, parmesan', price: 48.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1476124369491-e7addf5db371?w=500', badge: null, dietary: ['V'] },
  { id: 4, category: 'mains', name: isArabic.value ? 'دجاج مشوي بالليمون' : 'Poulet Rôti au Citron', description: isArabic.value ? 'دجاج بلدي، ليمون، زعتر، بطاطا مشوية' : 'Poulet fermier, citron, thym, pommes de terre', price: 38.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1532550907401-a500c9a57435?w=500', badge: null, dietary: ['GF'] },

  // 🥗 مقبلات
  { id: 5, category: 'starters', name: isArabic.value ? 'بريك بالدجاج' : 'Brik au Poulet', description: isArabic.value ? 'ورقة ملسوقة، دجاج، بقدونس، صفار بيض' : 'Feuille de malsouka, poulet, persil, jaune d\'œuf', price: 12.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1546069901-ba9599a7e63c?w=500', badge: 'lastThree', dietary: [] },
  { id: 6, category: 'starters', name: isArabic.value ? 'سلطة مشوية' : 'Slata Mechouia', description: isArabic.value ? 'فلفل مشوي، طماطم، ثوم، تونة' : 'Poivrons grillés, tomates, ail, thon', price: 10.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1512621776951-a57141f2eefd?w=500', badge: null, dietary: ['GF'] },
  { id: 7, category: 'starters', name: isArabic.value ? 'بوراتا وطماطم' : 'Burrata & Tomates', description: isArabic.value ? 'بوراتا كريمي، طماطم كرزية، ريحان' : 'Burrata crémeuse, tomates cerises, basilic', price: 22.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1608897013039-887f21d8c804?w=500', badge: null, dietary: ['V', 'GF'] },
  { id: 8, category: 'starters', name: isArabic.value ? 'كارباتشيو لحم' : 'Carpaccio de Bœuf', description: isArabic.value ? 'لحم بقري مقطع رقيق، بارميزان، جرجير' : 'Bœuf finement tranché, parmesan, roquette', price: 26.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1544025162-d76694265947?w=500', badge: null, dietary: ['GF'] },

  // 🍝 معكرونة
  { id: 9, category: 'pasta', name: isArabic.value ? 'باستا كاربونارا' : 'Pasta Carbonara', description: isArabic.value ? 'سباغيتي، بيض، جبن، فلفل أسود' : 'Spaghetti, œuf, fromage, poivre noir', price: 32.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1621996346565-e3dbc646d9a9?w=500', badge: null, dietary: [] },
  { id: 10, category: 'pasta', name: isArabic.value ? 'بيني أرابياتا' : 'Penne Arrabiata', description: isArabic.value ? 'معكرونة، صوص طماطم حار، ثوم' : 'Penne, sauce tomate piquante, ail', price: 28.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1608897013039-887f21d8c804?w=500', badge: null, dietary: ['V'] },
  { id: 11, category: 'pasta', name: isArabic.value ? 'فيتوتشيني ألفريدو' : 'Fettuccine Alfredo', description: isArabic.value ? 'معكرونة، كريمة، بارميزان' : 'Fettuccine, crème, parmesan', price: 35.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1621996346565-e3dbc646d9a9?w=500', badge: null, dietary: ['V'] },
  { id: 12, category: 'pasta', name: isArabic.value ? 'لازانيا' : 'Lasagna', description: isArabic.value ? 'طبقات لحم، بشاميل، جبن' : 'Couches de viande, béchamel, fromage', price: 38.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1574894709920-11b28e7367e3?w=500', badge: null, dietary: [] },

  // 🍖 مشاوي
  { id: 13, category: 'grill', name: isArabic.value ? 'ستيك ريب آي' : 'Ribeye Steak', description: isArabic.value ? 'لحم بقري معتق، صوص فلفل، بطاطا' : 'Bœuf maturé, sauce poivre, pommes de terre', price: 68.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1600891964092-4316c288032e?w=500', badge: 'chef', dietary: ['GF'] },
  { id: 14, category: 'grill', name: isArabic.value ? 'مشاوي مختلطة' : 'Mixed Grill', description: isArabic.value ? 'لحم، دجاج، كفتة، خضار مشوية' : 'Viande, poulet, kefta, légumes grillés', price: 52.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1544025162-d76694265947?w=500', badge: null, dietary: ['GF'] },
  { id: 15, category: 'grill', name: isArabic.value ? 'ضلوع الغنم' : 'Côtelettes d\'Agneau', description: isArabic.value ? 'ضلوع غنم، إكليل الجبل، ثوم' : 'Côtelettes d\'agneau, romarin, ail', price: 58.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1544025162-d76694265947?w=500', badge: null, dietary: ['GF'] },
  { id: 16, category: 'grill', name: isArabic.value ? 'دجاج مشوي كامل' : 'Poulet Grillé Entier', description: isArabic.value ? 'دجاج بلدي مشوي، ليمون، أعشاب' : 'Poulet fermier grillé, citron, herbes', price: 36.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1598103442097-8b74394b95c6?w=500', badge: null, dietary: ['GF'] },

  // 🍰 حلويات
  { id: 17, category: 'desserts', name: isArabic.value ? 'فوندان الشوكولاتة' : 'Fondant Chocolat', description: isArabic.value ? 'شوكولاتة داكنة سائلة، آيس كريم' : 'Chocolat noir coulant, glace vanille', price: 14.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1606313564200-e75d5e30476c?w=500', badge: null, dietary: ['V'] },
  { id: 18, category: 'desserts', name: isArabic.value ? 'تيراميسو' : 'Tiramisu', description: isArabic.value ? 'إسبريسو، ماسكاربوني، كاكاو' : 'Espresso, mascarpone, cacao', price: 12.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1571877227200-a0d98ea607e9?w=500', badge: null, dietary: ['V'] },
  { id: 19, category: 'desserts', name: isArabic.value ? 'بوفار باللوز' : 'Boufar aux Amandes', description: isArabic.value ? 'كعكة لوز، ماء ورد' : 'Gâteau aux amandes, eau de rose', price: 8.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1558961363-fa8fdf82db35?w=500', badge: null, dietary: ['V'] },
  { id: 20, category: 'desserts', name: isArabic.value ? 'كريم بروليه' : 'Crème Brûlée', description: isArabic.value ? 'كريمة فانيليا، سكر محروق' : 'Crème vanille, sucre caramélisé', price: 13.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1470124182917-cc6e71b22ecc?w=500', badge: null, dietary: ['V'] },

  // 🥤 مشروبات
  { id: 21, category: 'drinks', name: isArabic.value ? 'ماء معدني' : 'Eau Minérale', description: isArabic.value ? 'قارورة 75 سل' : 'Bouteille 75cl', price: 3.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1548839140-29a749e1cf4d?w=500', badge: null, dietary: ['V', 'GF'] },
  { id: 22, category: 'drinks', name: isArabic.value ? 'مشروب غازي' : 'Soda', description: isArabic.value ? 'كوكاكولا، فانتا، سبرايت' : 'Coca, Fanta, Sprite', price: 4.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1581636625402-29b2a704ef13?w=500', badge: null, dietary: ['V', 'GF'] },
  { id: 23, category: 'drinks', name: isArabic.value ? 'عصير طازج' : 'Jus Frais', description: isArabic.value ? 'برتقال، ليمون، فراولة' : 'Orange, citron, fraise', price: 6.0, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1600271886742-f049cd451bba?w=500', badge: null, dietary: ['V', 'GF'] },
  { id: 24, category: 'drinks', name: isArabic.value ? 'قهوة' : 'Café', description: isArabic.value ? 'إسبريسو، كابتشينو، قهوة تركية' : 'Espresso, cappuccino, café turc', price: 4.5, currency: isArabic.value ? 'د.ت' : 'DT', image: 'https://images.unsplash.com/photo-1510707577719-ae7c14805e3a?w=500', badge: null, dietary: ['V'] },
])

const filteredItems = computed(() => menuItems.value.filter(i => i.category === activeCategory.value))

function badgeLabel(badge) {
  return t.value[badge] || ''
}

const badgeClasses = {
  chef: 'text-[#C9A227] border-[#C9A227] bg-black/60',
  lastThree: 'text-[#FF6B6B] border-[#FF6B6B] bg-black/60',
}

// ===== Scroll handlers =====
let scrollFrame = null
function handleScroll() {
  if (scrollFrame !== null) return
  scrollFrame = window.requestAnimationFrame(() => {
    showBackToTop.value = window.scrollY > 400
    scrollFrame = null
  })
}
function scrollToTop() {
  window.scrollTo({
    top: 0,
    behavior: window.matchMedia('(prefers-reduced-motion: reduce)').matches ? 'auto' : 'smooth',
  })
}

onMounted(() => window.addEventListener('scroll', handleScroll, { passive: true }))
onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
  if (scrollFrame) cancelAnimationFrame(scrollFrame)
})
</script>

<template>
  <div :dir="isArabic ? 'rtl' : 'ltr'" class="min-h-screen bg-[#0F0F0F] text-[#EDEDED] font-sans selection:bg-[#C9A227] selection:text-black relative">
    
    <!-- Sticky Language Switcher -->
    <div class="fixed top-4 end-4 z-50">
      <button
        @click="emit('change-language')"
        class="flex items-center gap-1.5 px-3 py-1.5 rounded-full bg-white/10 backdrop-blur-sm text-white text-xs font-medium border border-white/20 hover:bg-[#C9A227] hover:text-black transition-colors shadow-lg"
      >
        <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" stroke-width="1.7" viewBox="0 0 24 24"><circle cx="12" cy="12" r="9" /><path d="M3 12h18M12 3a15 15 0 0 1 0 18M12 3a15 15 0 0 0 0 18" /></svg>
        {{ t.langButton }}
      </button>
    </div>

    <!-- HERO -->
    <section class="relative h-[60vh] min-h-[480px] overflow-hidden">
      <img src="https://images.unsplash.com/photo-1414235077428-338989a2e8c0?w=1600" :alt="restaurantInfo.name" class="absolute inset-0 w-full h-full object-cover opacity-70" />
      <div class="absolute inset-0 bg-gradient-to-t from-[#0F0F0F] via-[#0F0F0F]/60 to-transparent"></div>
      <div class="relative h-full flex flex-col items-center justify-center text-center px-6">
        <p class="text-[10px] tracking-[0.5em] text-[#C9A227] uppercase mb-4">{{ restaurantInfo.tagline }}</p>
        <h1 class="text-6xl sm:text-8xl font-serif text-white tracking-[0.1em]">{{ restaurantInfo.name }}</h1>
        <p class="text-xs text-gray-500 mt-4 tracking-[0.3em]">EST. {{ restaurantInfo.established }}</p>
        <div class="flex items-center gap-3 mt-6 text-xs text-white/60"><span>⭐ {{ restaurantInfo.rating }} ({{ restaurantInfo.reviews }})</span></div>
      </div>
    </section>

    <!-- INFO BAR -->
    <div class="border-y border-[#2A2A2A] py-4 px-6">
      <div class="max-w-6xl mx-auto flex flex-wrap items-center justify-center gap-6 text-[11px] text-gray-400 tracking-wide">
        <span>{{ restaurantInfo.address }}</span>
        <span class="text-[#C9A227]">{{ restaurantInfo.phone }}</span>
        <span>{{ restaurantInfo.hours }}</span>
      </div>
    </div>

    <!-- TABS -->
    <nav class="sticky top-0 z-20 bg-[#0F0F0F]/95 backdrop-blur-sm border-b border-[#2A2A2A]">
      <div class="max-w-6xl mx-auto flex justify-center gap-8 overflow-x-auto px-6 py-4 scrollbar-hide">
        <button v-for="cat in categories" :key="cat.id" class="shrink-0 text-xs tracking-[0.2em] uppercase transition-all pb-2 border-b" :class="activeCategory === cat.id ? 'text-[#C9A227] border-[#C9A227]' : 'text-gray-500 border-transparent hover:text-gray-300'" @click="activeCategory = cat.id">{{ cat.name }}</button>
      </div>
    </nav>

    <!-- MENU GRID -->
    <main class="max-w-6xl mx-auto px-4 sm:px-6 py-10 pb-24">
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 sm:gap-5">
        <article v-for="item in filteredItems" :key="item.id" class="group bg-[#141414] rounded-lg overflow-hidden border border-white/5 hover:border-[#C9A227]/30 transition-colors duration-300 flex flex-col">
          <div class="relative aspect-[4/3] overflow-hidden bg-[#1A1A1A]">
            <img :src="item.image" :alt="item.name" class="w-full h-full object-cover opacity-90 transition-all duration-500 group-hover:opacity-100 group-hover:scale-105" loading="lazy" />
            <span v-if="item.badge" class="absolute top-2 start-2 text-[9px] font-bold border px-2 py-0.5 tracking-wider uppercase backdrop-blur-sm" :class="badgeClasses[item.badge]">{{ badgeLabel(item.badge) }}</span>
          </div>
          <div class="p-4 flex flex-col flex-1">
            <h3 class="text-sm font-serif text-white leading-tight mb-1">{{ item.name }}</h3>
            <p class="text-xs text-gray-500 leading-relaxed line-clamp-2 mb-2">{{ item.description }}</p>
            <div v-if="item.dietary?.length" class="flex gap-1 mb-2">
              <span v-for="tag in item.dietary" :key="tag" class="text-[9px] font-medium px-1.5 py-0.5 rounded bg-white/5 text-gray-400">
                {{ tag === 'V' ? t.vegetarian : t.glutenFree }}
              </span>
            </div>
            <div class="mt-auto flex items-baseline justify-between">
              <span class="text-base font-light text-[#C9A227]">{{ item.price }} <span class="text-xs">{{ item.currency }}</span></span>
            </div>
          </div>
        </article>
      </div>
      <p class="text-center text-[10px] text-gray-600 mt-10 tracking-wide">{{ restaurantInfo.name }} · {{ restaurantInfo.instagram }} · {{ t.priceNote }}</p>
    </main>

    <!-- Footer info -->
    <footer class="border-t border-[#2A2A2A] py-8 px-6">
      <div class="max-w-6xl mx-auto grid grid-cols-1 sm:grid-cols-3 gap-6 text-center sm:text-start">
        <div>
          <p class="text-[10px] tracking-[0.3em] text-[#C9A227] uppercase mb-2">{{ t.address }}</p>
          <p class="text-xs text-gray-400">{{ restaurantInfo.address }}</p>
        </div>
        <div>
          <p class="text-[10px] tracking-[0.3em] text-[#C9A227] uppercase mb-2">{{ t.phone }}</p>
          <p class="text-xs text-gray-400" dir="ltr">{{ restaurantInfo.phone }}</p>
        </div>
        <div>
          <p class="text-[10px] tracking-[0.3em] text-[#C9A227] uppercase mb-2">{{ t.hours }}</p>
          <p class="text-xs text-gray-400">{{ restaurantInfo.hours }}</p>
        </div>
      </div>
      <p class="text-center text-[10px] text-gray-600 mt-6 tracking-wide">© {{ restaurantInfo.established }} {{ restaurantInfo.name }} · {{ t.followUs }} {{ restaurantInfo.instagram }}</p>
    </footer>

    <!-- Back to Top -->
    <Transition enter-active-class="transition duration-300 ease-out" enter-from-class="translate-y-3 opacity-0" enter-to-class="translate-y-0 opacity-100" leave-active-class="transition duration-200 ease-in" leave-from-class="translate-y-0 opacity-100" leave-to-class="translate-y-3 opacity-0">
      <button v-if="showBackToTop" @click="scrollToTop" class="fixed bottom-6 end-6 z-40 flex h-11 w-11 items-center justify-center rounded-full bg-[#C9A227] text-black shadow-lg hover:bg-white transition-colors duration-300 focus:outline-none focus:ring-2 focus:ring-[#C9A227] focus:ring-offset-2 focus:ring-offset-[#0F0F0F]" :aria-label="t.backToTop">
        <svg class="h-5 w-5" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M12 19V5m0 0-7 7m7-7 7 7" /></svg>
      </button>
    </Transition>
  </div>
</template>

<style scoped>
.scrollbar-hide::-webkit-scrollbar {
  display: none;
}
.scrollbar-hide {
  -ms-overflow-style: none;
  scrollbar-width: none;
}
</style>