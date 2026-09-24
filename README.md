# <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>سوبر ماركت أونلاين | كل حاجة توصلك</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;500;600;700;800&display=swap');
    body { font-family: 'Cairo', sans-serif; }
    .product-card:hover { transform: translateY(-4px); box-shadow: 0 12px 24px -8px rgba(0,0,0,0.15); }
    .cart-badge { animation: pulse 2s infinite; }
    @keyframes pulse { 0%,100%{transform:scale(1)} 50%{transform:scale(1.1)} }
    .modal-backdrop { background: rgba(0,0,0,0.5); backdrop-filter: blur(4px); }
    .scrollbar-hide::-webkit-scrollbar { display: none; }
    .gradient-bg { background: linear-gradient(135deg, #059669 0%, #10b981 50%, #34d399 100%); }
    .admin-panel { max-height: 80vh; overflow-y: auto; }
  </style>
</head>
<body class="bg-gray-50 text-gray-800 min-h-screen">

  <!-- Header -->
  <header class="gradient-bg text-white sticky top-0 z-50 shadow-lg">
    <div class="container mx-auto px-4 py-3">
      <div class="flex items-center justify-between gap-4">
        <div class="flex items-center gap-3">
          <div class="bg-white/20 p-2 rounded-xl">
            <i class="fas fa-shopping-basket text-2xl"></i>
          </div>
          <div>
            <h1 class="text-xl md:text-2xl font-bold">سوبر ماركت أونلاين</h1>
            <p class="text-xs opacity-90 hidden sm:block">كل حاجة توصلك لحد البيت</p>
          </div>
        </div>
        
        <div class="flex items-center gap-2 md:gap-4">
          <button onclick="openSearch()" class="bg-white/20 hover:bg-white/30 p-2.5 rounded-full transition">
            <i class="fas fa-search"></i>
          </button>
          <button onclick="toggleCart()" class="relative bg-white/20 hover:bg-white/30 p-2.5 rounded-full transition">
            <i class="fas fa-shopping-cart"></i>
            <span id="cartCount" class="absolute -top-1 -right-1 bg-red-500 text-white text-xs w-5 h-5 rounded-full flex items-center justify-center font-bold cart-badge hidden">0</span>
          </button>
          <button onclick="openAdmin()" class="bg-white/20 hover:bg-white/30 p-2.5 rounded-full transition" title="لوحة التحكم">
            <i class="fas fa-user-shield"></i>
          </button>
        </div>
      </div>
      
      <!-- Search Bar (hidden by default) -->
      <div id="searchBar" class="hidden mt-3">
        <input type="text" id="searchInput" placeholder="ابحث عن منتج..." 
               class="w-full px-4 py-2.5 rounded-xl text-gray-800 focus:outline-none focus:ring-2 focus:ring-white/50"
               oninput="filterProducts()">
      </div>
    </div>
  </header>

  <!-- Categories -->
  <section class="bg-white border-b sticky top-[72px] z-40 shadow-sm">
    <div class="container mx-auto px-4 py-3">
      <div id="categoriesBar" class="flex gap-2 overflow-x-auto scrollbar-hide pb-1">
        <!-- Categories loaded by JS -->
      </div>
    </div>
  </section>

  <!-- Main Content -->
  <main class="container mx-auto px-4 py-6">
    <!-- Hero Banner -->
    <div class="gradient-bg rounded-2xl p-6 md:p-8 mb-8 text-white relative overflow-hidden">
      <div class="relative z-10">
        <h2 class="text-2xl md:text-3xl font-bold mb-2">مرحباً بيك في سوبر ماركتنا! 🛒</h2>
        <p class="opacity-90 mb-4">اطلب كل اللي محتاجه وهيوصلك لحد باب البيت • حط لينك موقعك وهنتواصل معاك</p>
        <div class="flex flex-wrap gap-3 text-sm">
          <span class="bg-white/20 px-3 py-1 rounded-full"><i class="fas fa-truck ml-1"></i> توصيل سريع</span>
          <span class="bg-white/20 px-3 py-1 rounded-full"><i class="fas fa-tags ml-1"></i> أسعار منافسة</span>
          <span class="bg-white/20 px-3 py-1 rounded-full"><i class="fas fa-shield-alt ml-1"></i> جودة مضمونة</span>
        </div>
      </div>
      <div class="absolute left-0 bottom-0 opacity-10 text-9xl">
        <i class="fas fa-shopping-cart"></i>
      </div>
    </div>

    <!-- Products Grid -->
    <div id="productsGrid" class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4 md:gap-6">
      <!-- Products loaded by JS -->
    </div>

    <!-- Empty State -->
    <div id="emptyState" class="hidden text-center py-16">
      <i class="fas fa-box-open text-6xl text-gray-300 mb-4"></i>
      <h3 class="text-xl font-bold text-gray-500">مفيش منتجات هنا</h3>
      <p class="text-gray-400 mt-2">ادخل كأدمن وأضف منتجاتك</p>
    </div>
  </main>

  <!-- Cart Sidebar -->
  <div id="cartOverlay" class="fixed inset-0 bg-black/40 z-50 hidden" onclick="toggleCart()"></div>
  <aside id="cartSidebar" class="fixed top-0 left-0 h-full w-full max-w-md bg-white shadow-2xl z-50 transform -translate-x-full transition-transform duration-300 flex flex-col">
    <div class="p-4 border-b flex items-center justify-between bg-emerald-600 text-white">
      <h2 class="text-lg font-bold"><i class="fas fa-shopping-cart ml-2"></i> عربة التسوق</h2>
      <button onclick="toggleCart()" class="p-2 hover:bg-white/20 rounded-full"><i class="fas fa-times"></i></button>
    </div>
    <div id="cartItems" class="flex-1 overflow-y-auto p-4 space-y-3">
      <!-- Cart items -->
    </div>
    <div class="p-4 border-t bg-gray-50">
      <div class="flex justify-between text-lg font-bold mb-4">
        <span>المجموع:</span>
        <span id="cartTotal">0 ج.م</span>
      </div>
      <button onclick="openCheckout()" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white py-3 rounded-xl font-bold transition disabled:opacity-50" id="checkoutBtn" disabled>
        إتمام الطلب <i class="fas fa-arrow-left mr-2"></i>
      </button>
    </div>
  </aside>

  <!-- Checkout Modal -->
  <div id="checkoutModal" class="fixed inset-0 z-[60] hidden items-center justify-center p-4 modal-backdrop">
    <div class="bg-white rounded-2xl w-full max-w-lg max-h-[90vh] overflow-y-auto shadow-2xl">
      <div class="p-5 border-b flex items-center justify-between sticky top-0 bg-white">
        <h2 class="text-xl font-bold text-emerald-700"><i class="fas fa-clipboard-list ml-2"></i> إتمام الطلب</h2>
        <button onclick="closeCheckout()" class="p-2 hover:bg-gray-100 rounded-full"><i class="fas fa-times"></i></button>
      </div>
      <div class="p-5 space-y-4">
        <div>
          <label class="block text-sm font-medium mb-1">الاسم *</label>
          <input type="text" id="customerName" class="w-full border rounded-xl px-4 py-2.5 focus:ring-2 focus:ring-emerald-500 focus:outline-none" placeholder="اسمك بالكامل">
        </div>
        <div>
          <label class="block text-sm font-medium mb-1">رقم الموبايل *</label>
          <input type="tel" id="customerPhone" class="w-full border rounded-xl px-4 py-2.5 focus:ring-2 focus:ring-emerald-500 focus:outline-none" placeholder="01xxxxxxxxx">
        </div>
        <div>
          <label class="block text-sm font-medium mb-1">العنوان التفصيلي *</label>
          <textarea id="customerAddress" rows="2" class="w-full border rounded-xl px-4 py-2.5 focus:ring-2 focus:ring-emerald-500 focus:outline-none" placeholder="الشارع، المبنى، الدور، علامة مميزة..."></textarea>
        </div>
        
        <div>
          <label class="block text-sm font-medium mb-1">
            <i class="fas fa-map-marker-alt text-emerald-600 ml-1"></i> لينك الموقع (جوجل مابس) *
          </label>
          <input type="url" id="customerLocationLink" class="w-full border rounded-xl px-4 py-2.5 focus:ring-2 focus:ring-emerald-500 focus:outline-none" 
                 placeholder="الصق لينك الموقع من جوجل مابس هنا">
          <p class="text-xs text-gray-500 mt-1">افتح جوجل مابس ← شارك الموقع ← انسخ اللينك والصقه هنا</p>
        </div>

        <div class="bg-gray-100 rounded-xl p-4">
          <div class="flex justify-between mb-1">
            <span>قيمة المنتجات:</span>
            <span id="subtotalDisplay">0 ج.م</span>
          </div>
          <div class="flex justify-between text-sm text-gray-500 mb-1">
            <span>التوصيل:</span>
            <span>هيتحسب بعد استلام الطلب</span>
          </div>
          <div class="flex justify-between text-lg font-bold text-emerald-700 border-t pt-2 mt-2">
            <span>إجمالي المنتجات:</span>
            <span id="grandTotalDisplay">0 ج.م</span>
          </div>
        </div>

        <button onclick="sendToWhatsApp()" id="sendWhatsAppBtn" class="w-full bg-green-600 hover:bg-green-700 text-white py-3.5 rounded-xl font-bold text-lg transition flex items-center justify-center gap-2 disabled:opacity-50" disabled>
          <i class="fab fa-whatsapp text-2xl"></i> إرسال الطلب على واتساب
        </button>
        <p class="text-xs text-center text-gray-500">هيتبعت الفاتورة + لينك الموقع على: 01208028426</p>
      </div>
    </div>
  </div>

  <!-- Admin Login Modal -->
  <div id="adminLoginModal" class="fixed inset-0 z-[70] hidden items-center justify-center p-4 modal-backdrop">
    <div class="bg-white rounded-2xl w-full max-w-sm shadow-2xl p-6">
      <h2 class="text-xl font-bold text-center mb-4"><i class="fas fa-lock ml-2 text-emerald-600"></i> دخول الأدمن</h2>
      <input type="password" id="adminPassword" class="w-full border rounded-xl px-4 py-3 mb-4 focus:ring-2 focus:ring-emerald-500 focus:outline-none" placeholder="كلمة المرور" onkeydown="if(event.key==='Enter')loginAdmin()">
      <button onclick="loginAdmin()" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white py-3 rounded-xl font-bold">دخول</button>
      <button onclick="closeAdminLogin()" class="w-full mt-2 text-gray-500 py-2">إلغاء</button>
      <p class="text-xs text-center text-gray-400 mt-3">الافتراضي</p>
    </div>
  </div>

  <!-- Admin Panel Modal -->
  <div id="adminPanelModal" class="fixed inset-0 z-[70] hidden items-center justify-center p-4 modal-backdrop">
    <div class="bg-white rounded-2xl w-full max-w-4xl max-h-[90vh] overflow-hidden shadow-2xl flex flex-col">
      <div class="p-4 border-b flex items-center justify-between bg-emerald-700 text-white">
        <h2 class="text-lg font-bold"><i class="fas fa-cogs ml-2"></i> لوحة تحكم الأدمن</h2>
        <div class="flex gap-2">
          <button onclick="logoutAdmin()" class="bg-white/20 hover:bg-white/30 px-3 py-1.5 rounded-lg text-sm">خروج</button>
          <button onclick="closeAdminPanel()" class="p-2 hover:bg-white/20 rounded-full"><i class="fas fa-times"></i></button>
        </div>
      </div>
      
      <div class="flex border-b">
        <button onclick="showAdminTab('products')" id="tabProducts" class="flex-1 py-3 font-medium border-b-2 border-emerald-600 text-emerald-700">المنتجات</button>
        <button onclick="showAdminTab('categories')" id="tabCategories" class="flex-1 py-3 font-medium text-gray-500">الأقسام</button>
        <button onclick="showAdminTab('settings')" id="tabSettings" class="flex-1 py-3 font-medium text-gray-500">الإعدادات</button>
      </div>
      
      <div class="admin-panel p-4 flex-1 overflow-y-auto">
        <!-- Products Tab -->
        <div id="adminProducts" class="space-y-4">
          <div class="bg-emerald-50 rounded-xl p-4">
            <h3 class="font-bold mb-3">إضافة / تعديل منتج</h3>
            <input type="hidden" id="editProductId">
            <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
              <input type="text" id="prodName" class="border rounded-xl px-3 py-2" placeholder="اسم المنتج *">
              <input type="number" id="prodPrice" class="border rounded-xl px-3 py-2" placeholder="السعر (ج.م) *" step="0.5" min="0">
              <select id="prodCategory" class="border rounded-xl px-3 py-2"></select>
              <input type="text" id="prodUnit" class="border rounded-xl px-3 py-2" placeholder="الوحدة (كيلو، علبة، قطعة...)">
              
              <!-- Image section -->
              <div class="md:col-span-2 space-y-2">
                <label class="block text-sm font-medium">صورة المنتج</label>
                <div class="flex flex-col sm:flex-row gap-3 items-start">
                  <div id="imagePreview" class="w-24 h-24 bg-gray-100 rounded-xl border-2 border-dashed border-gray-300 flex items-center justify-center overflow-hidden flex-shrink-0">
                    <i class="fas fa-image text-2xl text-gray-400"></i>
                  </div>
                  <div class="flex-1 space-y-2 w-full">
                    <input type="file" id="prodImageFile" accept="image/*" class="w-full text-sm file:mr-3 file:py-2 file:px-4 file:rounded-xl file:border-0 file:bg-emerald-100 file:text-emerald-700 file:font-medium hover:file:bg-emerald-200" onchange="handleImageUpload(event)">
                    <input type="url" id="prodImage" class="w-full border rounded-xl px-3 py-2 text-sm" placeholder="أو الصق رابط صورة مباشر (اختياري)">
                    <p class="text-xs text-gray-500">يفضل صورة مربعة • الحد الأقصى للرفع 300 كيلوبايت</p>
                  </div>
                </div>
              </div>
              
              <textarea id="prodDesc" class="border rounded-xl px-3 py-2 md:col-span-2" rows="2" placeholder="وصف مختصر (اختياري)"></textarea>
            </div>
            <div class="flex gap-2 mt-3">
              <button onclick="saveProduct()" class="bg-emerald-600 hover:bg-emerald-700 text-white px-5 py-2 rounded-xl font-medium">حفظ المنتج</button>
              <button onclick="clearProductForm()" class="bg-gray-200 hover:bg-gray-300 px-4 py-2 rounded-xl">مسح</button>
            </div>
          </div>
          
          <div id="adminProductsList" class="space-y-2">
            <!-- List of products for admin -->
          </div>
        </div>
        
        <!-- Categories Tab -->
        <div id="adminCategories" class="hidden space-y-4">
          <div class="bg-emerald-50 rounded-xl p-4">
            <h3 class="font-bold mb-3">إضافة قسم جديد</h3>
            <div class="flex gap-2">
              <input type="text" id="newCategory" class="flex-1 border rounded-xl px-3 py-2" placeholder="اسم القسم">
              <button onclick="addCategory()" class="bg-emerald-600 hover:bg-emerald-700 text-white px-5 py-2 rounded-xl">إضافة</button>
            </div>
          </div>
          <div id="adminCategoriesList" class="space-y-2"></div>
        </div>
        
        <!-- Settings Tab -->
        <div id="adminSettings" class="hidden space-y-4">
          <div class="bg-emerald-50 rounded-xl p-4 space-y-3">
            <h3 class="font-bold">إعدادات المتجر</h3>
            <div>
              <label class="block text-sm mb-1">اسم المتجر</label>
              <input type="text" id="storeName" class="w-full border rounded-xl px-3 py-2">
            </div>
            <div>
              <label class="block text-sm mb-1">رقم الواتساب (بدون صفر، مع كود الدولة)</label>
              <input type="text" id="whatsappNumber" class="w-full border rounded-xl px-3 py-2" placeholder="201208028426">
            </div>
            <div>
              <label class="block text-sm mb-1">كلمة مرور الأدمن الجديدة</label>
              <input type="password" id="newAdminPass" class="w-full border rounded-xl px-3 py-2" placeholder="اتركها فاضية لو مش عايز تغير">
            </div>
            <button onclick="saveSettings()" class="bg-emerald-600 hover:bg-emerald-700 text-white px-5 py-2 rounded-xl font-medium">حفظ الإعدادات</button>
          </div>
          
          <div class="bg-red-50 border border-red-200 rounded-xl p-4">
            <h3 class="font-bold text-red-700 mb-2">منطقة خطر</h3>
            <button onclick="resetAllData()" class="bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded-xl text-sm">مسح كل البيانات وإعادة الضبط</button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Toast Notification -->
  <div id="toast" class="fixed bottom-6 left-1/2 -translate-x-1/2 bg-gray-900 text-white px-6 py-3 rounded-full shadow-lg z-[100] hidden transition-all opacity-0">
    <span id="toastMsg"></span>
  </div>

  <script>
    // ========== DEFAULT DATA ==========
    const DEFAULT_CATEGORIES = ['مواد غذائية', 'مشروبات', 'منظفات وعناية', 'خضروات وفاكهة', 'ألبان وأجبان', 'لحوم ودواجن', 'مخبوزات', 'حلويات وسناكس'];
    
    const DEFAULT_PRODUCTS = [
      { id: 1, name: 'أرز بسمتي 1 كيلو', price: 45, category: 'مواد غذائية', unit: 'كيلو', image: '', desc: 'أرز بسمتي فاخر' },
      { id: 2, name: 'زيت عباد الشمس 1 لتر', price: 55, category: 'مواد غذائية', unit: 'لتر', image: '', desc: '' },
      { id: 3, name: 'سكر أبيض 1 كيلو', price: 28, category: 'مواد غذائية', unit: 'كيلو', image: '', desc: '' },
      { id: 4, name: 'مياه معدنية 1.5 لتر', price: 7, category: 'مشروبات', unit: 'زجاجة', image: '', desc: '' },
      { id: 5, name: 'كوكاكولا 1 لتر', price: 18, category: 'مشروبات', unit: 'زجاجة', image: '', desc: '' },
      { id: 6, name: 'صابون سائل 1 لتر', price: 45, category: 'منظفات وعناية', unit: 'لتر', image: '', desc: '' },
      { id: 7, name: 'طماطم طازجة', price: 12, category: 'خضروات وفاكهة', unit: 'كيلو', image: '', desc: '' },
      { id: 8, name: 'بطاطس', price: 15, category: 'خضروات وفاكهة', unit: 'كيلو', image: '', desc: '' },
      { id: 9, name: 'لبن جهينة 1 لتر', price: 32, category: 'ألبان وأجبان', unit: 'لتر', image: '', desc: '' },
      { id: 10, name: 'جبنة بيضاء 500 جم', price: 40, category: 'ألبان وأجبان', unit: 'علبة', image: '', desc: '' },
    ];

    const DEFAULT_SETTINGS = {
      storeName: 'سوبر ماركت أونلاين',
      whatsapp: '201208028426',
      adminPassword: 'admin123'
    };

    // ========== STATE ==========
    let products = [];
    let categories = [];
    let cart = [];
    let settings = {};
    let isAdmin = false;
    let currentCategory = 'الكل';

    // ========== INIT ==========
    function init() {
      loadData();
      renderCategories();
      renderProducts();
      renderCart();
      updateCartBadge();
    }

    function loadData() {
      products = JSON.parse(localStorage.getItem('sm_products')) || [...DEFAULT_PRODUCTS];
      categories = JSON.parse(localStorage.getItem('sm_categories')) || [...DEFAULT_CATEGORIES];
      cart = JSON.parse(localStorage.getItem('sm_cart')) || [];
      settings = JSON.parse(localStorage.getItem('sm_settings')) || {...DEFAULT_SETTINGS};
      // Ensure defaults
      if (!settings.adminPassword) settings.adminPassword = 'admin123';
      if (!settings.whatsapp) settings.whatsapp = '201208028426';
    }

    function saveData() {
      localStorage.setItem('sm_products', JSON.stringify(products));
      localStorage.setItem('sm_categories', JSON.stringify(categories));
      localStorage.setItem('sm_cart', JSON.stringify(cart));
      localStorage.setItem('sm_settings', JSON.stringify(settings));
    }

    // ========== RENDER ==========
    function renderCategories() {
      const bar = document.getElementById('categoriesBar');
      const allCats = ['الكل', ...categories];
      bar.innerHTML = allCats.map(cat => `
        <button onclick="selectCategory('${cat}')" 
                class="whitespace-nowrap px-4 py-2 rounded-full text-sm font-medium transition
                       ${currentCategory === cat ? 'bg-emerald-600 text-white' : 'bg-gray-100 hover:bg-gray-200 text-gray-700'}">
          ${cat}
        </button>
      `).join('');
    }

    function selectCategory(cat) {
      currentCategory = cat;
      renderCategories();
      renderProducts();
    }

    function renderProducts() {
      const grid = document.getElementById('productsGrid');
      const empty = document.getElementById('emptyState');
      let filtered = products;
      
      if (currentCategory !== 'الكل') {
        filtered = filtered.filter(p => p.category === currentCategory);
      }
      
      const search = document.getElementById('searchInput')?.value?.toLowerCase() || '';
      if (search) {
        filtered = filtered.filter(p => p.name.toLowerCase().includes(search) || (p.desc||'').toLowerCase().includes(search));
      }

      if (filtered.length === 0) {
        grid.innerHTML = '';
        empty.classList.remove('hidden');
        return;
      }
      empty.classList.add('hidden');

      grid.innerHTML = filtered.map(p => `
        <div class="product-card bg-white rounded-2xl shadow-sm border overflow-hidden transition duration-300">
          <div class="aspect-square bg-gray-100 relative overflow-hidden">
            ${p.image ? 
              `<img src="${p.image}" alt="${p.name}" class="w-full h-full object-cover" onerror="this.parentElement.innerHTML='<div class=\\'flex items-center justify-center h-full text-4xl text-gray-300\\'><i class=\\'fas fa-box\\'></i></div>'">` :
              `<div class="flex items-center justify-center h-full text-5xl text-gray-300"><i class="fas fa-box"></i></div>`
            }
          </div>
          <div class="p-3">
            <h3 class="font-bold text-sm md:text-base line-clamp-2 mb-1">${p.name}</h3>
            <p class="text-xs text-gray-500 mb-2">${p.unit || ''}</p>
            <div class="flex items-center justify-between">
              <span class="text-emerald-600 font-bold text-lg">${p.price} <span class="text-xs">ج.م</span></span>
              <button onclick="addToCart(${p.id})" class="bg-emerald-600 hover:bg-emerald-700 text-white w-9 h-9 rounded-full flex items-center justify-center transition">
                <i class="fas fa-plus"></i>
              </button>
            </div>
          </div>
        </div>
      `).join('');
    }

    function filterProducts() {
      renderProducts();
    }

    function openSearch() {
      const bar = document.getElementById('searchBar');
      bar.classList.toggle('hidden');
      if (!bar.classList.contains('hidden')) {
        document.getElementById('searchInput').focus();
      }
    }

    // ========== CART ==========
    function addToCart(id) {
      const product = products.find(p => p.id === id);
      if (!product) return;
      const existing = cart.find(c => c.id === id);
      if (existing) {
        existing.qty += 1;
      } else {
        cart.push({ ...product, qty: 1 });
      }
      saveData();
      renderCart();
      updateCartBadge();
      showToast(`تمت إضافة ${product.name} للسلة`);
    }

    function changeQty(id, delta) {
      const item = cart.find(c => c.id === id);
      if (!item) return;
      item.qty += delta;
      if (item.qty <= 0) {
        cart = cart.filter(c => c.id !== id);
      }
      saveData();
      renderCart();
      updateCartBadge();
    }

    function removeFromCart(id) {
      cart = cart.filter(c => c.id !== id);
      saveData();
      renderCart();
      updateCartBadge();
    }

    function renderCart() {
      const container = document.getElementById('cartItems');
      const totalEl = document.getElementById('cartTotal');
      const btn = document.getElementById('checkoutBtn');
      
      if (cart.length === 0) {
        container.innerHTML = `
          <div class="text-center py-12 text-gray-400">
            <i class="fas fa-shopping-cart text-5xl mb-3"></i>
            <p>السلة فاضية</p>
          </div>`;
        totalEl.textContent = '0 ج.م';
        btn.disabled = true;
        return;
      }

      let total = 0;
      container.innerHTML = cart.map(item => {
        const sub = item.price * item.qty;
        total += sub;
        return `
          <div class="flex gap-3 bg-gray-50 rounded-xl p-3">
            <div class="w-16 h-16 bg-gray-200 rounded-lg flex items-center justify-center text-2xl text-gray-400 flex-shrink-0">
              ${item.image ? `<img src="${item.image}" class="w-full h-full object-cover rounded-lg">` : '<i class="fas fa-box"></i>'}
            </div>
            <div class="flex-1 min-w-0">
              <h4 class="font-medium text-sm line-clamp-1">${item.name}</h4>
              <p class="text-emerald-600 font-bold text-sm">${item.price} ج.م</p>
              <div class="flex items-center gap-2 mt-1">
                <button onclick="changeQty(${item.id}, -1)" class="w-7 h-7 bg-gray-200 rounded-full text-sm">-</button>
                <span class="font-bold w-6 text-center">${item.qty}</span>
                <button onclick="changeQty(${item.id}, 1)" class="w-7 h-7 bg-emerald-100 text-emerald-700 rounded-full text-sm">+</button>
                <button onclick="removeFromCart(${item.id})" class="mr-auto text-red-500 text-sm"><i class="fas fa-trash"></i></button>
              </div>
            </div>
          </div>`;
      }).join('');
      
      totalEl.textContent = total.toFixed(2) + ' ج.م';
      btn.disabled = false;
    }

    function updateCartBadge() {
      const count = cart.reduce((s, i) => s + i.qty, 0);
      const badge = document.getElementById('cartCount');
      if (count > 0) {
        badge.textContent = count > 99 ? '99+' : count;
        badge.classList.remove('hidden');
      } else {
        badge.classList.add('hidden');
      }
    }

    function toggleCart() {
      const sidebar = document.getElementById('cartSidebar');
      const overlay = document.getElementById('cartOverlay');
      const isOpen = !sidebar.classList.contains('-translate-x-full');
      if (isOpen) {
        sidebar.classList.add('-translate-x-full');
        overlay.classList.add('hidden');
      } else {
        sidebar.classList.remove('-translate-x-full');
        overlay.classList.remove('hidden');
        renderCart();
      }
    }

    // ========== CHECKOUT & LOCATION ==========
    function openCheckout() {
      if (cart.length === 0) return;
      toggleCart();
      document.getElementById('checkoutModal').classList.remove('hidden');
      document.getElementById('checkoutModal').classList.add('flex');
      updateCheckoutTotals();
    }

    function closeCheckout() {
      document.getElementById('checkoutModal').classList.add('hidden');
      document.getElementById('checkoutModal').classList.remove('flex');
    }

    function updateCheckoutTotals() {
      const subtotal = cart.reduce((s, i) => s + i.price * i.qty, 0);
      document.getElementById('subtotalDisplay').textContent = subtotal.toFixed(2) + ' ج.م';
      document.getElementById('grandTotalDisplay').textContent = subtotal.toFixed(2) + ' ج.م';
      
      const btn = document.getElementById('sendWhatsAppBtn');
      const name = document.getElementById('customerName').value.trim();
      const phone = document.getElementById('customerPhone').value.trim();
      const address = document.getElementById('customerAddress').value.trim();
      const locationLink = document.getElementById('customerLocationLink').value.trim();
      btn.disabled = !(name && phone && address && locationLink);
    }

    // Listen for input changes
    ['customerName','customerPhone','customerAddress','customerLocationLink'].forEach(id => {
      document.getElementById(id)?.addEventListener('input', updateCheckoutTotals);
    });

    function sendToWhatsApp() {
      const name = document.getElementById('customerName').value.trim();
      const phone = document.getElementById('customerPhone').value.trim();
      const address = document.getElementById('customerAddress').value.trim();
      const locationLink = document.getElementById('customerLocationLink').value.trim();

      if (!name || !phone || !address || !locationLink) {
        showToast('املأ كل البيانات بما فيها لينك الموقع');
        return;
      }

      const subtotal = cart.reduce((s, i) => s + i.price * i.qty, 0);

      let msg = `🛒 *طلب جديد من سوبر ماركت أونلاين*\n`;
      msg += `━━━━━━━━━━━━━━\n`;
      msg += `👤 *العميل:* ${name}\n`;
      msg += `📱 *الموبايل:* ${phone}\n`;
      msg += `📍 *العنوان:* ${address}\n`;
      msg += `🔗 *لينك الموقع:*\n${locationLink}\n`;
      msg += `━━━━━━━━━━━━━━\n`;
      msg += `*تفاصيل الطلب:*\n\n`;

      cart.forEach((item, i) => {
        msg += `${i+1}. ${item.name}\n`;
        msg += `   الكمية: ${item.qty} × ${item.price} = ${(item.qty * item.price).toFixed(2)} ج.م\n\n`;
      });

      msg += `━━━━━━━━━━━━━━\n`;
      msg += `💰 *قيمة المنتجات:* ${subtotal.toFixed(2)} ج.م\n`;
      msg += `🚚 *التوصيل:* هيتحسب بعد استلام الطلب\n`;
      msg += `━━━━━━━━━━━━━━\n`;
      msg += `اضغط على لينك الموقع عشان تشوف مكان العميل 👆`;

      const encoded = encodeURIComponent(msg);
      const waNumber = settings.whatsapp || '201208028426';
      window.open(`https://wa.me/${waNumber}?text=${encoded}`, '_blank');
      
      showToast('تم فتح واتساب بالفاتورة 🎉');
    }

    // ========== ADMIN ==========
    function openAdmin() {
      if (isAdmin) {
        openAdminPanel();
      } else {
        document.getElementById('adminLoginModal').classList.remove('hidden');
        document.getElementById('adminLoginModal').classList.add('flex');
        document.getElementById('adminPassword').value = '';
        document.getElementById('adminPassword').focus();
      }
    }

    function closeAdminLogin() {
      document.getElementById('adminLoginModal').classList.add('hidden');
      document.getElementById('adminLoginModal').classList.remove('flex');
    }

    function loginAdmin() {
      const pass = document.getElementById('adminPassword').value;
      if (pass === settings.adminPassword) {
        isAdmin = true;
        closeAdminLogin();
        openAdminPanel();
        showToast('مرحباً أدمن! 👋');
      } else {
        showToast('كلمة المرور غلط');
      }
    }

    function logoutAdmin() {
      isAdmin = false;
      closeAdminPanel();
      showToast('تم تسجيل الخروج');
    }

    function openAdminPanel() {
      document.getElementById('adminPanelModal').classList.remove('hidden');
      document.getElementById('adminPanelModal').classList.add('flex');
      showAdminTab('products');
      populateCategorySelect();
      renderAdminProducts();
      renderAdminCategories();
      loadSettingsForm();
    }

    function closeAdminPanel() {
      document.getElementById('adminPanelModal').classList.add('hidden');
      document.getElementById('adminPanelModal').classList.remove('flex');
    }

    function showAdminTab(tab) {
      ['products','categories','settings'].forEach(t => {
        document.getElementById('admin' + t.charAt(0).toUpperCase() + t.slice(1)).classList.add('hidden');
        document.getElementById('tab' + t.charAt(0).toUpperCase() + t.slice(1)).classList.remove('border-emerald-600','text-emerald-700');
        document.getElementById('tab' + t.charAt(0).toUpperCase() + t.slice(1)).classList.add('text-gray-500');
      });
      document.getElementById('admin' + tab.charAt(0).toUpperCase() + tab.slice(1)).classList.remove('hidden');
      document.getElementById('tab' + tab.charAt(0).toUpperCase() + tab.slice(1)).classList.add('border-emerald-600','text-emerald-700');
      document.getElementById('tab' + tab.charAt(0).toUpperCase() + tab.slice(1)).classList.remove('text-gray-500');
    }

    function populateCategorySelect() {
      const sel = document.getElementById('prodCategory');
      sel.innerHTML = categories.map(c => `<option value="${c}">${c}</option>`).join('');
    }

    // Image handling
    function handleImageUpload(event) {
      const file = event.target.files[0];
      if (!file) return;
      
      if (!file.type.startsWith('image/')) {
        showToast('اختار ملف صورة فقط');
        return;
      }
      
      // Limit 300KB
      if (file.size > 300 * 1024) {
        showToast('الصورة كبيرة أوي (أقصى 300 كيلوبايت). صغرها الأول');
        event.target.value = '';
        return;
      }

      const reader = new FileReader();
      reader.onload = function(e) {
        const base64 = e.target.result;
        document.getElementById('prodImage').value = base64;
        updateImagePreview(base64);
        showToast('تم رفع الصورة بنجاح');
      };
      reader.readAsDataURL(file);
    }

    function updateImagePreview(src) {
      const preview = document.getElementById('imagePreview');
      if (src) {
        preview.innerHTML = `<img src="${src}" class="w-full h-full object-cover">`;
      } else {
        preview.innerHTML = `<i class="fas fa-image text-2xl text-gray-400"></i>`;
      }
    }

    // Also update preview when pasting URL
    document.getElementById('prodImage')?.addEventListener('input', function() {
      updateImagePreview(this.value.trim());
    });

    function saveProduct() {
      const id = document.getElementById('editProductId').value;
      const name = document.getElementById('prodName').value.trim();
      const price = parseFloat(document.getElementById('prodPrice').value);
      const category = document.getElementById('prodCategory').value;
      const unit = document.getElementById('prodUnit').value.trim();
      const image = document.getElementById('prodImage').value.trim();
      const desc = document.getElementById('prodDesc').value.trim();

      if (!name || isNaN(price) || price < 0) {
        showToast('اكتب اسم وسعر صحيح');
        return;
      }

      if (id) {
        // Edit
        const idx = products.findIndex(p => p.id == id);
        if (idx !== -1) {
          products[idx] = { ...products[idx], name, price, category, unit, image, desc };
        }
      } else {
        // New
        const newId = products.length ? Math.max(...products.map(p => p.id)) + 1 : 1;
        products.push({ id: newId, name, price, category, unit, image, desc });
      }
      saveData();
      clearProductForm();
      renderAdminProducts();
      renderProducts();
      showToast('تم حفظ المنتج');
    }

    function clearProductForm() {
      document.getElementById('editProductId').value = '';
      document.getElementById('prodName').value = '';
      document.getElementById('prodPrice').value = '';
      document.getElementById('prodUnit').value = '';
      document.getElementById('prodImage').value = '';
      document.getElementById('prodImageFile').value = '';
      document.getElementById('prodDesc').value = '';
      updateImagePreview('');
    }

    function editProduct(id) {
      const p = products.find(x => x.id === id);
      if (!p) return;
      document.getElementById('editProductId').value = p.id;
      document.getElementById('prodName').value = p.name;
      document.getElementById('prodPrice').value = p.price;
      document.getElementById('prodCategory').value = p.category;
      document.getElementById('prodUnit').value = p.unit || '';
      document.getElementById('prodImage').value = p.image || '';
      document.getElementById('prodDesc').value = p.desc || '';
      updateImagePreview(p.image || '');
      showAdminTab('products');
      // Scroll to form
      document.getElementById('adminProducts').scrollIntoView({ behavior: 'smooth' });
    }

    function deleteProduct(id) {
      if (!confirm('متأكد تحذف المنتج؟')) return;
      products = products.filter(p => p.id !== id);
      cart = cart.filter(c => c.id !== id);
      saveData();
      renderAdminProducts();
      renderProducts();
      renderCart();
      updateCartBadge();
      showToast('تم الحذف');
    }

    function renderAdminProducts() {
      const list = document.getElementById('adminProductsList');
      if (products.length === 0) {
        list.innerHTML = '<p class="text-gray-400 text-center py-4">مفيش منتجات</p>';
        return;
      }
      list.innerHTML = products.map(p => `
        <div class="flex items-center gap-3 bg-white border rounded-xl p-3">
          <div class="w-12 h-12 bg-gray-100 rounded-lg flex items-center justify-center text-xl text-gray-400 flex-shrink-0">
            ${p.image ? `<img src="${p.image}" class="w-full h-full object-cover rounded-lg">` : '<i class="fas fa-box"></i>'}
          </div>
          <div class="flex-1 min-w-0">
            <h4 class="font-medium text-sm truncate">${p.name}</h4>
            <p class="text-xs text-gray-500">${p.category} • ${p.price} ج.م</p>
          </div>
          <button onclick="editProduct(${p.id})" class="text-blue-600 p-2"><i class="fas fa-edit"></i></button>
          <button onclick="deleteProduct(${p.id})" class="text-red-500 p-2"><i class="fas fa-trash"></i></button>
        </div>
      `).join('');
    }

    function addCategory() {
      const name = document.getElementById('newCategory').value.trim();
      if (!name) return showToast('اكتب اسم القسم');
      if (categories.includes(name)) return showToast('القسم موجود بالفعل');
      categories.push(name);
      saveData();
      document.getElementById('newCategory').value = '';
      renderAdminCategories();
      renderCategories();
      populateCategorySelect();
      showToast('تم إضافة القسم');
    }

    function deleteCategory(name) {
      if (!confirm(`حذف قسم "${name}"؟ المنتجات هتفضل بس بدون قسم واضح.`)) return;
      categories = categories.filter(c => c !== name);
      saveData();
      renderAdminCategories();
      renderCategories();
      populateCategorySelect();
      showToast('تم الحذف');
    }

    function renderAdminCategories() {
      const list = document.getElementById('adminCategoriesList');
      list.innerHTML = categories.map(c => `
        <div class="flex items-center justify-between bg-white border rounded-xl p-3">
          <span class="font-medium">${c}</span>
          <button onclick="deleteCategory('${c}')" class="text-red-500 p-2"><i class="fas fa-trash"></i></button>
        </div>
      `).join('');
    }

    function loadSettingsForm() {
      document.getElementById('storeName').value = settings.storeName || '';
      document.getElementById('whatsappNumber').value = settings.whatsapp || '';
      document.getElementById('newAdminPass').value = '';
    }

    function saveSettings() {
      settings.storeName = document.getElementById('storeName').value.trim() || 'سوبر ماركت أونلاين';
      settings.whatsapp = document.getElementById('whatsappNumber').value.trim().replace(/[^0-9]/g, '') || '201208028426';
      const newPass = document.getElementById('newAdminPass').value;
      if (newPass) settings.adminPassword = newPass;
      saveData();
      showToast('تم حفظ الإعدادات');
    }

    function resetAllData() {
      if (!confirm('هتمسح كل المنتجات والسلة والإعدادات! متأكد؟')) return;
      if (!confirm('تأكيد نهائي؟ مفيش رجوع.')) return;
      localStorage.removeItem('sm_products');
      localStorage.removeItem('sm_categories');
      localStorage.removeItem('sm_cart');
      localStorage.removeItem('sm_settings');
      isAdmin = false;
      closeAdminPanel();
      init();
      showToast('تم إعادة الضبط');
    }

    // ========== UTILS ==========
    function showToast(msg) {
      const toast = document.getElementById('toast');
      document.getElementById('toastMsg').textContent = msg;
      toast.classList.remove('hidden');
      setTimeout(() => toast.classList.remove('opacity-0'), 10);
      setTimeout(() => {
        toast.classList.add('opacity-0');
        setTimeout(() => toast.classList.add('hidden'), 300);
      }, 2500);
    }

    // Start
    init();
  </script>
</body>
</html>
