[index.html](https://github.com/user-attachments/files/33780203/index.html)
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Trading Journal - Focus on What Matters</title>
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Lucide Icons -->
  <script src="https://unpkg.com/lucide@latest"></script>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;600&family=Plus+Jakarta+Sans:wght@400;500;600;700&display=swap');
    
    body {
      font-family: 'Plus Jakarta Sans', sans-serif;
    }
    .font-mono-numbers {
      font-family: 'JetBrains Mono', monospace;
    }
    /* Custom scrollbar for minimalist clean look */
    ::-webkit-scrollbar {
      width: 6px;
      height: 6px;
    }
    ::-webkit-scrollbar-track {
      background: #0f172a;
    }
    ::-webkit-scrollbar-thumb {
      background: #334155;
      border-radius: 9999px;
    }
    ::-webkit-scrollbar-thumb:hover {
      background: #475569;
    }
  </style>
</head>
<body class="bg-slate-950 text-slate-100 min-h-screen flex flex-col antialiased selection:bg-emerald-500/20 selection:text-emerald-300">

  <!-- Header -->
  <header class="border-b border-slate-800/80 bg-slate-900/60 backdrop-blur sticky top-0 z-40">
    <div class="max-w-6xl mx-auto px-4 sm:px-6 py-4 flex flex-wrap items-center justify-between gap-4">
      <div class="flex items-center space-x-3">
        <div class="h-10 w-10 rounded-xl bg-gradient-to-tr from-emerald-500 to-cyan-500 flex items-center justify-center shadow-lg shadow-emerald-500/20">
          <i data-lucide="line-chart" class="w-5 h-5 text-slate-950 stroke-[2.5]"></i>
        </div>
        <div>
          <h1 class="text-xl font-bold tracking-tight text-white flex items-center gap-2">
            TradePulse <span class="text-xs font-semibold px-2 py-0.5 rounded-full bg-emerald-500/10 text-emerald-400 border border-emerald-500/20">Journal</span>
          </h1>
          <p class="text-xs text-slate-400">Log <span class="text-slate-200">What</span> you traded, <span class="text-slate-200">Why</span>, and the <span class="text-slate-200">Result</span></p>
        </div>
      </div>

      <!-- Quick Actions (Backup & Stats) -->
      <div class="flex items-center gap-2">
        <button id="exportCsvBtn" title="Export as CSV" class="text-xs flex items-center gap-1.5 px-3 py-2 rounded-lg bg-slate-800/80 hover:bg-slate-700/80 text-slate-200 border border-slate-700/60 transition shadow-sm">
          <i data-lucide="download" class="w-3.5 h-3.5"></i>
          <span class="hidden sm:inline">Export CSV</span>
        </button>
        <button id="openNewTradeModal" class="text-xs font-semibold flex items-center gap-1.5 px-3.5 py-2 rounded-lg bg-emerald-500 hover:bg-emerald-400 text-slate-950 transition shadow-md shadow-emerald-500/25 active:scale-95">
          <i data-lucide="plus-circle" class="w-4 h-4"></i>
          <span>Log New Trade</span>
        </button>
      </div>
    </div>
  </header>

  <!-- Main Content -->
  <main class="max-w-6xl mx-auto px-4 sm:px-6 py-6 w-full flex-1">
    
    <!-- Top Summary Banner -->
    <div class="grid grid-cols-2 sm:grid-cols-4 gap-3 sm:gap-4 mb-6">
      <div class="bg-slate-900/50 border border-slate-800/80 rounded-xl p-4">
        <span class="text-xs text-slate-400 font-medium">Total Recorded</span>
        <div id="statTotalTrades" class="text-2xl font-bold text-white mt-1 font-mono-numbers">0</div>
      </div>
      <div class="bg-slate-900/50 border border-slate-800/80 rounded-xl p-4">
        <span class="text-xs text-emerald-400 font-medium">Win Rate</span>
        <div id="statWinRate" class="text-2xl font-bold text-emerald-400 mt-1 font-mono-numbers">0%</div>
      </div>
      <div class="bg-slate-900/50 border border-slate-800/80 rounded-xl p-4">
        <span class="text-xs text-slate-400 font-medium">Net P&amp;L</span>
        <div id="statNetPnl" class="text-2xl font-bold text-slate-200 mt-1 font-mono-numbers">$0.00</div>
      </div>
      <div class="bg-slate-900/50 border border-slate-800/80 rounded-xl p-4">
        <span class="text-xs text-slate-400 font-medium">Win / Loss Ratio</span>
        <div id="statWinLossRatio" class="text-2xl font-bold text-slate-200 mt-1 font-mono-numbers">0W / 0L</div>
      </div>
    </div>

    <!-- Filter & Search Bar -->
    <div class="bg-slate-900/70 border border-slate-800/80 rounded-xl p-3 mb-6 flex flex-col md:flex-row gap-3 items-stretch md:items-center justify-between">
      <div class="flex-1 relative">
        <i data-lucide="search" class="w-4 h-4 text-slate-400 absolute left-3 top-3"></i>
        <input 
          type="text" 
          id="searchInput" 
          placeholder="Search by ticker (e.g. BTC, AAPL, NIFTY) or reasons..." 
          class="w-full pl-9 pr-4 py-2 bg-slate-950/80 border border-slate-800 rounded-lg text-sm text-slate-200 placeholder-slate-500 focus:outline-none focus:border-emerald-500/60 transition"
        />
      </div>
      <div class="flex items-center gap-2 overflow-x-auto pb-1 md:pb-0">
        <button data-filter="ALL" class="filter-btn text-xs px-3 py-1.5 rounded-lg bg-emerald-500/20 text-emerald-400 border border-emerald-500/40 font-medium transition">All</button>
        <button data-filter="WIN" class="filter-btn text-xs px-3 py-1.5 rounded-lg bg-slate-800 text-slate-300 hover:bg-slate-700/60 transition">Wins</button>
        <button data-filter="LOSS" class="filter-btn text-xs px-3 py-1.5 rounded-lg bg-slate-800 text-slate-300 hover:bg-slate-700/60 transition">Losses</button>
        <button data-filter="BREAKEVEN" class="filter-btn text-xs px-3 py-1.5 rounded-lg bg-slate-800 text-slate-300 hover:bg-slate-700/60 transition">Break-Even</button>
        <button data-filter="OPEN" class="filter-btn text-xs px-3 py-1.5 rounded-lg bg-slate-800 text-slate-300 hover:bg-slate-700/60 transition">Open</button>
      </div>
    </div>

    <!-- Trades Feed -->
    <div id="tradesListContainer" class="space-y-4">
      <!-- Trades will be rendered here via JavaScript -->
    </div>

    <!-- Empty State -->
    <div id="emptyState" class="hidden text-center py-16 px-4 bg-slate-900/30 border border-dashed border-slate-800 rounded-2xl">
      <div class="w-14 h-14 mx-auto rounded-full bg-slate-800/80 flex items-center justify-center text-slate-400 mb-3">
        <i data-lucide="book-open" class="w-7 h-7"></i>
      </div>
      <h3 class="text-base font-semibold text-slate-200">No trades recorded yet</h3>
      <p class="text-xs text-slate-400 max-w-sm mx-auto mt-1 mb-5">
        Consistency comes from reflection. Log which trade you took, your setup conviction, and the final result.
      </p>
      <button id="emptyStateAddBtn" class="text-xs font-semibold inline-flex items-center gap-2 px-4 py-2.5 rounded-lg bg-emerald-500 hover:bg-emerald-400 text-slate-950 transition shadow-lg shadow-emerald-500/20">
        <i data-lucide="plus" class="w-4 h-4"></i>
        Log Your First Trade
      </button>
    </div>

  </main>

  <!-- Modal / Slide-over Form for Entry -->
  <div id="tradeModal" class="fixed inset-0 z-50 flex items-center justify-center bg-black/70 backdrop-blur-sm p-4 hidden opacity-0 transition-opacity duration-200">
    <div id="modalContent" class="bg-slate-900 border border-slate-800 w-full max-w-2xl rounded-2xl shadow-2xl overflow-hidden flex flex-col max-h-[90vh] transform scale-95 transition-transform duration-200">
      
      <!-- Modal Header -->
      <div class="px-6 py-4 border-b border-slate-800 flex items-center justify-between bg-slate-900/90">
        <div class="flex items-center gap-2.5">
          <div class="p-2 rounded-lg bg-emerald-500/10 text-emerald-400">
            <i data-lucide="pen-tool" class="w-4 h-4"></i>
          </div>
          <div>
            <h2 id="modalTitle" class="text-base font-bold text-white">Log Trading Journal Entry</h2>
            <p class="text-xs text-slate-400">Capture trade identity, reasoning, and outcome</p>
          </div>
        </div>
        <button id="closeModalBtn" class="text-slate-400 hover:text-white p-1.5 rounded-lg hover:bg-slate-800 transition">
          <i data-lucide="x" class="w-5 h-5"></i>
        </button>
      </div>

      <!-- Form Body (Scrollable) -->
      <form id="tradeForm" class="p-6 space-y-6 overflow-y-auto">
        <input type="hidden" id="editTradeId" value="">

        <!-- SECTION 1: WHICH TRADE WAS TAKEN -->
        <div class="bg-slate-950/50 p-4 rounded-xl border border-slate-800/80 space-y-4">
          <div class="flex items-center gap-2 text-xs font-semibold uppercase tracking-wider text-emerald-400">
            <span class="w-5 h-5 rounded-full bg-emerald-500/20 text-emerald-400 flex items-center justify-center text-[10px]">1</span>
            Which Trade Did You Take?
          </div>

          <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
            <div>
              <label class="block text-xs font-medium text-slate-300 mb-1">Asset / Symbol / Ticker *</label>
              <input 
                type="text" 
                id="formSymbol" 
                required 
                placeholder="e.g. BTCUSDT, NIFTY, AAPL, EURUSD" 
                class="w-full px-3 py-2 bg-slate-900 border border-slate-700/80 rounded-lg text-sm text-white placeholder-slate-500 focus:outline-none focus:border-emerald-500"
              />
            </div>

            <div>
              <label class="block text-xs font-medium text-slate-300 mb-1">Trade Direction *</label>
              <div class="grid grid-cols-2 gap-2">
                <label class="cursor-pointer">
                  <input type="radio" name="direction" value="BUY" checked class="peer sr-only">
                  <div class="text-center py-2 text-xs font-semibold rounded-lg border border-slate-700 peer-checked:bg-emerald-500/20 peer-checked:border-emerald-500 peer-checked:text-emerald-400 text-slate-300 transition">
                    🟢 BUY / LONG
                  </div>
                </label>
                <label class="cursor-pointer">
                  <input type="radio" name="direction" value="SELL" class="peer sr-only">
                  <div class="text-center py-2 text-xs font-semibold rounded-lg border border-slate-700 peer-checked:bg-rose-500/20 peer-checked:border-rose-500 peer-checked:text-rose-400 text-slate-300 transition">
                    🔴 SELL / SHORT
                  </div>
                </label>
              </div>
            </div>

            <div>
              <label class="block text-xs font-medium text-slate-300 mb-1">Entry Price (Optional)</label>
              <input 
                type="number" 
                step="any" 
                id="formEntryPrice" 
                placeholder="e.g. 150.25" 
                class="w-full px-3 py-2 bg-slate-900 border border-slate-700/80 rounded-lg text-sm text-white placeholder-slate-500 focus:outline-none focus:border-emerald-500 font-mono-numbers"
              />
            </div>

            <div>
              <label class="block text-xs font-medium text-slate-300 mb-1">Exit Price (Optional)</label>
              <input 
                type="number" 
                step="any" 
                id="formExitPrice" 
                placeholder="e.g. 155.00" 
                class="w-full px-3 py-2 bg-slate-900 border border-slate-700/80 rounded-lg text-sm text-white placeholder-slate-500 focus:outline-none focus:border-emerald-500 font-mono-numbers"
              />
            </div>

            <div class="sm:col-span-2">
              <label class="block text-xs font-medium text-slate-300 mb-1">Date &amp; Time Taken</label>
              <input 
                type="datetime-local" 
                id="formDateTime" 
                class="w-full px-3 py-2 bg-slate-900 border border-slate-700/80 rounded-lg text-sm text-white focus:outline-none focus:border-emerald-500"
              />
            </div>
          </div>
        </div>

        <!-- SECTION 2: WHY DID YOU TAKE THIS TRADE -->
        <div class="bg-slate-950/50 p-4 rounded-xl border border-slate-800/80 space-y-4">
          <div class="flex items-center gap-2 text-xs font-semibold uppercase tracking-wider text-cyan-400">
            <span class="w-5 h-5 rounded-full bg-cyan-500/20 text-cyan-400 flex items-center justify-center text-[10px]">2</span>
            Why Did You Take This Trade? (Rationale &amp; Setup) *
          </div>

          <div>
            <textarea 
              id="formRationale" 
              required 
              rows="3" 
              placeholder="State your technical reason, indicator trigger, catalyst, or pattern (e.g. 15m breakout above key resistance with heavy volume; retest of 200 EMA; news catalyst)..." 
              class="w-full px-3 py-2 bg-slate-900 border border-slate-700/80 rounded-lg text-sm text-white placeholder-slate-500 focus:outline-none focus:border-cyan-500"
            ></textarea>
            <p class="text-[11px] text-slate-400 mt-1">Be honest with your emotions or rules followed vs broken.</p>
          </div>
        </div>

        <!-- SECTION 3: WHAT IS THE RESULT -->
        <div class="bg-slate-950/50 p-4 rounded-xl border border-slate-800/80 space-y-4">
          <div class="flex items-center gap-2 text-xs font-semibold uppercase tracking-wider text-amber-400">
            <span class="w-5 h-5 rounded-full bg-amber-500/20 text-amber-400 flex items-center justify-center text-[10px]">3</span>
            What Is The Result?
          </div>

          <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
            <div>
              <label class="block text-xs font-medium text-slate-300 mb-1">Outcome Status *</label>
              <select id="formOutcome" class="w-full px-3 py-2 bg-slate-900 border border-slate-700/80 rounded-lg text-sm text-white focus:outline-none focus:border-amber-500">
                <option value="WIN">🟢 WIN</option>
                <option value="LOSS">🔴 LOSS</option>
                <option value="BREAKEVEN">⚪ BREAK-EVEN</option>
                <option value="OPEN">🟡 STILL OPEN</option>
              </select>
            </div>

            <div>
              <label class="block text-xs font-medium text-slate-300 mb-1">P&amp;L Value / Amount ($ or pts)</label>
              <input 
                type="number" 
                step="any" 
                id="formPnl" 
                placeholder="+250 or -120" 
                class="w-full px-3 py-2 bg-slate-900 border border-slate-700/80 rounded-lg text-sm text-white placeholder-slate-500 focus:outline-none focus:border-amber-500 font-mono-numbers"
              />
            </div>

            <div class="sm:col-span-2">
              <label class="block text-xs font-medium text-slate-300 mb-1">Outcome Notes / Key Learnings</label>
              <textarea 
                id="formReviewNotes" 
                rows="2" 
                placeholder="What did you learn? Did you take profit on target or close early? Did you respect your stop-loss?" 
                class="w-full px-3 py-2 bg-slate-900 border border-slate-700/80 rounded-lg text-sm text-white placeholder-slate-500 focus:outline-none focus:border-amber-500"
              ></textarea>
            </div>
          </div>
        </div>

        <!-- Form Actions -->
        <div class="flex items-center justify-end gap-3 pt-2">
          <button type="button" id="cancelFormBtn" class="px-4 py-2 text-xs font-medium text-slate-300 hover:text-white bg-slate-800 rounded-lg hover:bg-slate-700 transition">
            Cancel
          </button>
          <button type="submit" class="px-5 py-2 text-xs font-semibold text-slate-950 bg-emerald-500 hover:bg-emerald-400 rounded-lg transition shadow-md shadow-emerald-500/20">
            Save Entry
          </button>
        </div>
      </form>
    </div>
  </div>

  <!-- Notification Toast -->
  <div id="toast" class="fixed bottom-5 right-5 z-50 bg-slate-800 border border-slate-700 text-slate-200 text-xs px-4 py-3 rounded-xl shadow-xl flex items-center gap-2 transform translate-y-16 opacity-0 transition-all duration-300 pointer-events-none">
    <i data-lucide="check" class="w-4 h-4 text-emerald-400"></i>
    <span id="toastMsg">Logged successfully</span>
  </div>

  <!-- App Script -->
  <script>
    // Storage Key
    const STORAGE_KEY = 'tradepulse_minimal_journal_v1';

    // State
    let trades = [];
    let currentFilter = 'ALL';
    let searchQuery = '';

    // Initialize DOM references
    const tradesListContainer = document.getElementById('tradesListContainer');
    const emptyState = document.getElementById('emptyState');
    const tradeModal = document.getElementById('tradeModal');
    const modalContent = document.getElementById('modalContent');
    const tradeForm = document.getElementById('tradeForm');
    const openModalBtn = document.getElementById('openNewTradeModal');
    const emptyStateAddBtn = document.getElementById('emptyStateAddBtn');
    const closeModalBtn = document.getElementById('closeModalBtn');
    const cancelFormBtn = document.getElementById('cancelFormBtn');
    const searchInput = document.getElementById('searchInput');
    const exportCsvBtn = document.getElementById('exportCsvBtn');

    // Stats Elements
    const statTotalTrades = document.getElementById('statTotalTrades');
    const statWinRate = document.getElementById('statWinRate');
    const statNetPnl = document.getElementById('statNetPnl');
    const statWinLossRatio = document.getElementById('statWinLossRatio');

    // Default sample initial entries if brand new
    const sampleInitialTrades = [
      {
        id: '1',
        symbol: 'BTCUSDT',
        direction: 'BUY',
        entryPrice: 62400,
        exitPrice: 64150,
        dateTime: new Date(Date.now() - 86400000 * 2).toISOString().slice(0, 16),
        rationale: 'Clean retest of the daily support level with a bullish engulfing candle on 4H. Volume spiked 2x higher than previous candles.',
        outcome: 'WIN',
        pnl: 1750,
        notes: 'Held strictly to 2R target. Executed setup according to rules.'
      },
      {
        id: '2',
        symbol: 'AAPL',
        direction: 'SELL',
        entryPrice: 182.50,
        exitPrice: 184.20,
        dateTime: new Date(Date.now() - 86400000).toISOString().slice(0, 16),
        rationale: 'Tried to fade the morning opening rally expecting an instant liquidity sweep below previous day high.',
        outcome: 'LOSS',
        pnl: -170,
        notes: 'Mistake: Front-ran the confirmation signal. Market had strong trend day momentum. Kept stop-loss intact though.'
      }
    ];

    // Initialize application
    function init() {
      const savedData = localStorage.getItem(STORAGE_KEY);
      if (savedData) {
        try {
          trades = JSON.parse(savedData);
        } catch (e) {
          trades = sampleInitialTrades;
        }
      } else {
        trades = sampleInitialTrades;
        persistTrades();
      }

      setupEventListeners();
      render();
      lucide.createIcons();
    }

    // Persist to local storage
    function persistTrades() {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(trades));
    }

    // Event listeners
    function setupEventListeners() {
      openModalBtn.addEventListener('click', () => openModal());
      emptyStateAddBtn.addEventListener('click', () => openModal());
      closeModalBtn.addEventListener('click', closeModal);
      cancelFormBtn.addEventListener('click', closeModal);

      // Close modal on backdrop click
      tradeModal.addEventListener('click', (e) => {
        if (e.target === tradeModal) closeModal();
      });

      // Escape key to close modal
      window.addEventListener('keydown', (e) => {
        if (e.key === 'Escape' && !tradeModal.classList.contains('hidden')) {
          closeModal();
        }
      });

      // Form submission
      tradeForm.addEventListener('submit', handleFormSubmit);

      // Search
      searchInput.addEventListener('input', (e) => {
        searchQuery = e.target.value.toLowerCase().trim();
        render();
      });

      // Filter tabs
      document.querySelectorAll('.filter-btn').forEach(btn => {
        btn.addEventListener('click', (e) => {
          document.querySelectorAll('.filter-btn').forEach(b => {
            b.classList.remove('bg-emerald-500/20', 'text-emerald-400', 'border-emerald-500/40');
            b.classList.add('bg-slate-800', 'text-slate-300');
          });
          const target = e.currentTarget;
          target.classList.add('bg-emerald-500/20', 'text-emerald-400', 'border-emerald-500/40');
          target.classList.remove('bg-slate-800', 'text-slate-300');

          currentFilter = target.getAttribute('data-filter');
          render();
        });
      });

      // CSV Export
      exportCsvBtn.addEventListener('click', exportToCsv);
    }

    // Modal Control
    function openModal(editTrade = null) {
      tradeForm.reset();
      
      const now = new Date();
      // Format current local time for datetime-local input
      const localDatetime = new Date(now.getTime() - now.getTimezoneOffset() * 60000).toISOString().slice(0, 16);

      if (editTrade) {
        document.getElementById('modalTitle').textContent = 'Edit Trading Journal Entry';
        document.getElementById('editTradeId').value = editTrade.id;
        document.getElementById('formSymbol').value = editTrade.symbol;
        
        // Direction radio
        const dirRadios = document.getElementsByName('direction');
        for (let r of dirRadios) {
          if (r.value === editTrade.direction) r.checked = true;
        }

        document.getElementById('formEntryPrice').value = editTrade.entryPrice ?? '';
        document.getElementById('formExitPrice').value = editTrade.exitPrice ?? '';
        document.getElementById('formDateTime').value = editTrade.dateTime || localDatetime;
        document.getElementById('formRationale').value = editTrade.rationale || '';
        document.getElementById('formOutcome').value = editTrade.outcome || 'WIN';
        document.getElementById('formPnl').value = editTrade.pnl ?? '';
        document.getElementById('formReviewNotes').value = editTrade.notes || '';
      } else {
        document.getElementById('modalTitle').textContent = 'Log Trading Journal Entry';
        document.getElementById('editTradeId').value = '';
        document.getElementById('formDateTime').value = localDatetime;
      }

      tradeModal.classList.remove('hidden');
      setTimeout(() => {
        tradeModal.classList.remove('opacity-0');
        modalContent.classList.remove('scale-95');
      }, 10);
    }

    function closeModal() {
      tradeModal.classList.add('opacity-0');
      modalContent.classList.add('scale-95');
      setTimeout(() => {
        tradeModal.classList.add('hidden');
      }, 200);
    }

    // Handle Form Submit
    function handleFormSubmit(e) {
      e.preventDefault();

      const editId = document.getElementById('editTradeId').value;
      const symbol = document.getElementById('formSymbol').value.trim().toUpperCase();
      const direction = document.querySelector('input[name="direction"]:checked').value;
      const entryPrice = document.getElementById('formEntryPrice').value ? parseFloat(document.getElementById('formEntryPrice').value) : null;
      const exitPrice = document.getElementById('formExitPrice').value ? parseFloat(document.getElementById('formExitPrice').value) : null;
      const dateTime = document.getElementById('formDateTime').value;
      const rationale = document.getElementById('formRationale').value.trim();
      const outcome = document.getElementById('formOutcome').value;
      const pnl = document.getElementById('formPnl').value !== '' ? parseFloat(document.getElementById('formPnl').value) : null;
      const notes = document.getElementById('formReviewNotes').value.trim();

      const tradeData = {
        id: editId || Date.now().toString(),
        symbol,
        direction,
        entryPrice,
        exitPrice,
        dateTime,
        rationale,
        outcome,
        pnl,
        notes
      };

      if (editId) {
        const index = trades.findIndex(t => t.id === editId);
        if (index !== -1) {
          trades[index] = tradeData;
          showToast('Trade entry updated successfully');
        }
      } else {
        trades.unshift(tradeData);
        showToast('New trade journal entry recorded');
      }

      persistTrades();
      closeModal();
      render();
    }

    // Delete trade
    window.deleteTrade = function(id) {
      // Direct action without disruptive alert dialog
      trades = trades.filter(t => t.id !== id);
      persistTrades();
      render();
      showToast('Trade entry deleted');
    };

    // Edit trade trigger
    window.editTrade = function(id) {
      const trade = trades.find(t => t.id === id);
      if (trade) {
        openModal(trade);
      }
    };

    // Render logic
    function render() {
      updateStats();

      // Filter trades
      const filtered = trades.filter(trade => {
        // Outcome filter
        if (currentFilter !== 'ALL' && trade.outcome !== currentFilter) {
          return false;
        }
        // Search filter
        if (searchQuery) {
          const matchSymbol = trade.symbol.toLowerCase().includes(searchQuery);
          const matchRationale = (trade.rationale || '').toLowerCase().includes(searchQuery);
          const matchNotes = (trade.notes || '').toLowerCase().includes(searchQuery);
          return matchSymbol || matchRationale || matchNotes;
        }
        return true;
      });

      if (filtered.length === 0) {
        tradesListContainer.innerHTML = '';
        emptyState.classList.remove('hidden');
        lucide.createIcons();
        return;
      }

      emptyState.classList.add('hidden');

      tradesListContainer.innerHTML = filtered.map(trade => {
        // Outcome styling
        let badgeColor = 'bg-slate-800 text-slate-300 border-slate-700';
        let badgeText = 'BREAK-EVEN';
        let pnlColor = 'text-slate-300';

        if (trade.outcome === 'WIN') {
          badgeColor = 'bg-emerald-500/10 text-emerald-400 border-emerald-500/30';
          badgeText = 'WIN';
          pnlColor = 'text-emerald-400';
        } else if (trade.outcome === 'LOSS') {
          badgeColor = 'bg-rose-500/10 text-rose-400 border-rose-500/30';
          badgeText = 'LOSS';
          pnlColor = 'text-rose-400';
        } else if (trade.outcome === 'OPEN') {
          badgeColor = 'bg-amber-500/10 text-amber-400 border-amber-500/30';
          badgeText = 'OPEN';
          pnlColor = 'text-amber-400';
        }

        const isBuy = trade.direction === 'BUY';
        const dirBadge = isBuy
          ? '<span class="px-2 py-0.5 rounded text-[11px] font-semibold bg-emerald-500/15 text-emerald-400 border border-emerald-500/20">LONG / BUY</span>'
          : '<span class="px-2 py-0.5 rounded text-[11px] font-semibold bg-rose-500/15 text-rose-400 border border-rose-500/20">SHORT / SELL</span>';

        const dateStr = trade.dateTime ? new Date(trade.dateTime).toLocaleString([], { dateStyle: 'medium', timeStyle: 'short' }) : 'Unknown Date';

        const pnlFormatted = trade.pnl !== null 
          ? (trade.pnl > 0 ? `+${trade.pnl.toLocaleString()}` : `${trade.pnl.toLocaleString()}`)
          : '--';

        return `
          <div class="bg-slate-900/60 border border-slate-800 hover:border-slate-700/80 rounded-2xl p-5 sm:p-6 transition shadow-sm space-y-4">
            
            <!-- Trade Header: Which Trade -->
            <div class="flex flex-wrap items-center justify-between gap-3 border-b border-slate-800/60 pb-3">
              <div class="flex items-center gap-3">
                <div class="text-lg font-bold tracking-tight text-white font-mono-numbers">
                  ${trade.symbol}
                </div>
                ${dirBadge}
                <span class="text-xs text-slate-400 flex items-center gap-1">
                  <i data-lucide="calendar" class="w-3.5 h-3.5"></i>
                  ${dateStr}
                </span>
              </div>

              <div class="flex items-center gap-2">
                <span class="px-2.5 py-1 text-xs font-bold rounded-lg border ${badgeColor}">
                  ${badgeText}
                </span>
                <span class="font-mono-numbers text-sm font-semibold ${pnlColor} ml-1">
                  ${pnlFormatted}
                </span>

                <!-- Options menu -->
                <div class="flex items-center gap-1 ml-2 pl-2 border-l border-slate-800">
                  <button onclick="editTrade('${trade.id}')" title="Edit entry" class="p-1.5 rounded-lg text-slate-400 hover:text-white hover:bg-slate-800 transition">
                    <i data-lucide="edit-3" class="w-3.5 h-3.5"></i>
                  </button>
                  <button onclick="deleteTrade('${trade.id}')" title="Delete entry" class="p-1.5 rounded-lg text-slate-400 hover:text-rose-400 hover:bg-rose-500/10 transition">
                    <i data-lucide="trash-2" class="w-3.5 h-3.5"></i>
                  </button>
                </div>
              </div>
            </div>

            <!-- Price Points if present -->
            ${(trade.entryPrice !== null || trade.exitPrice !== null) ? `
              <div class="flex items-center gap-6 text-xs text-slate-400 font-mono-numbers bg-slate-950/40 px-3.5 py-2 rounded-lg border border-slate-800/40">
                ${trade.entryPrice !== null ? `<div>Entry: <span class="text-slate-200 font-semibold">${trade.entryPrice}</span></div>` : ''}
                ${trade.exitPrice !== null ? `<div>Exit: <span class="text-slate-200 font-semibold">${trade.exitPrice}</span></div>` : ''}
              </div>
            ` : ''}

            <!-- 2 Core Questions: WHY & RESULT -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4 pt-1">
              
              <!-- 1. Why I took the trade -->
              <div class="bg-slate-950/60 p-4 rounded-xl border border-slate-800/70 flex flex-col justify-between">
                <div>
                  <div class="text-[11px] font-semibold uppercase tracking-wider text-cyan-400/90 mb-2 flex items-center gap-1.5">
                    <i data-lucide="help-circle" class="w-3.5 h-3.5"></i>
                    Why I Took This Trade
                  </div>
                  <p class="text-xs sm:text-sm text-slate-300 leading-relaxed whitespace-pre-line">
                    ${trade.rationale || '<span class="text-slate-500 italic">No rationale provided.</span>'}
                  </p>
                </div>
              </div>

              <!-- 2. What was the outcome & lessons -->
              <div class="bg-slate-950/60 p-4 rounded-xl border border-slate-800/70 flex flex-col justify-between">
                <div>
                  <div class="text-[11px] font-semibold uppercase tracking-wider text-amber-400/90 mb-2 flex items-center gap-1.5">
                    <i data-lucide="check-circle" class="w-3.5 h-3.5"></i>
                    What Happened / Key Takeaway
                  </div>
                  <p class="text-xs sm:text-sm text-slate-300 leading-relaxed whitespace-pre-line">
                    ${trade.notes || '<span class="text-slate-500 italic">No post-trade notes or review entered.</span>'}
                  </p>
                </div>
              </div>

            </div>
          </div>
        `;
      }).join('');

      lucide.createIcons();
    }

    // Update Statistics
    function updateStats() {
      const total = trades.length;
      statTotalTrades.textContent = total;

      const closedTrades = trades.filter(t => t.outcome === 'WIN' || t.outcome === 'LOSS');
      const wins = trades.filter(t => t.outcome === 'WIN').length;
      const losses = trades.filter(t => t.outcome === 'LOSS').length;

      const winRate = closedTrades.length > 0 ? Math.round((wins / closedTrades.length) * 100) : 0;
      statWinRate.textContent = `${winRate}%`;

      const netPnl = trades.reduce((acc, curr) => acc + (curr.pnl || 0), 0);
      const sign = netPnl > 0 ? '+' : '';
      statNetPnl.textContent = `${sign}$${netPnl.toLocaleString(undefined, { minimumFractionDigits: 2, maximumFractionDigits: 2 })}`;
      
      if (netPnl > 0) {
        statNetPnl.className = 'text-2xl font-bold text-emerald-400 mt-1 font-mono-numbers';
      } else if (netPnl < 0) {
        statNetPnl.className = 'text-2xl font-bold text-rose-400 mt-1 font-mono-numbers';
      } else {
        statNetPnl.className = 'text-2xl font-bold text-slate-200 mt-1 font-mono-numbers';
      }

      statWinLossRatio.textContent = `${wins}W / ${losses}L`;
    }

    // Toast notification
    function showToast(message) {
      const toast = document.getElementById('toast');
      const toastMsg = document.getElementById('toastMsg');
      toastMsg.textContent = message;
      toast.classList.remove('translate-y-16', 'opacity-0');
      setTimeout(() => {
        toast.classList.add('translate-y-16', 'opacity-0');
      }, 2500);
    }

    // CSV Export
    function exportToCsv() {
      if (trades.length === 0) {
        showToast('No trades to export');
        return;
      }

      const headers = ['ID', 'Symbol', 'Direction', 'EntryPrice', 'ExitPrice', 'DateTime', 'Rationale (Why)', 'Outcome', 'PnL', 'ReviewNotes'];
      const rows = trades.map(t => [
        t.id,
        `"${(t.symbol || '').replace(/"/g, '""')}"`,
        t.direction,
        t.entryPrice ?? '',
        t.exitPrice ?? '',
        t.dateTime,
        `"${(t.rationale || '').replace(/"/g, '""')}"`,
        t.outcome,
        t.pnl ?? '',
        `"${(t.notes || '').replace(/"/g, '""')}"`
      ]);

      const csvContent = "data:text/csv;charset=utf-8," + [headers.join(','), ...rows.map(e => e.join(','))].join('\n');
      const encodedUri = encodeURI(csvContent);
      const link = document.createElement('a');
      link.setAttribute('href', encodedUri);
      link.setAttribute('download', `trade_journal_${new Date().toISOString().slice(0, 10)}.csv`);
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
      showToast('Exported journal to CSV');
    }

    // Start app
    window.addEventListener('DOMContentLoaded', init);
  </script>
</body>
</html>
