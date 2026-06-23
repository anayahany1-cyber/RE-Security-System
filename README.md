[re_security_system.tsx](https://github.com/user-attachments/files/29228959/re_security_system.tsx)
import React, { useState, useEffect } from 'react';
import { 
  Shield, ShieldCheck, ShieldAlert, Lock, Unlock, Key, 
  Server, Smartphone, Monitor, Database, AlertTriangle, 
  CheckCircle, XCircle, Info, Activity, Fingerprint,
  ChevronRight, Download, Moon, Sun, Menu, X, Users,
  BarChart3, Settings, PlayCircle, BookOpen, Home,
  Sparkles, MessageSquare, Loader2, Send
} from 'lucide-react';

const callGeminiAPI = async (prompt) => {
  const apiKey = "";
  const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`;
  const payload = {
    contents: [{ parts: [{ text: prompt }] }]
  };

  for (let i = 0; i < 5; i++) {
    try {
      const response = await fetch(url, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload)
      });
      if (!response.ok) throw new Error('API Error');
      const data = await response.json();
      return data.candidates?.[0]?.content?.parts?.[0]?.text || "Maaf, Chaperone sedang tidak dapat memproses informasi.";
    } catch (error) {
      if (i === 4) throw new Error("Gagal terhubung ke jaringan AI. Silakan coba lagi nanti.");
      const delay = Math.pow(2, i) * 1000;
      await new Promise(resolve => setTimeout(resolve, delay));
    }
  }
};

const App = () => {
  const [darkMode, setDarkMode] = useState(false);
  const [activeTab, setActiveTab] = useState('home');
  const [isMobileMenuOpen, setIsMobileMenuOpen] = useState(false);

  useEffect(() => {
    if (darkMode) {
      document.documentElement.classList.add('dark');
    } else {
      document.documentElement.classList.remove('dark');
    }
  }, [darkMode]);

  const toggleDarkMode = () => setDarkMode(!darkMode);

  const navigateTo = (tab) => {
    setActiveTab(tab);
    setIsMobileMenuOpen(false);
    window.scrollTo({ top: 0, behavior: 'smooth' });
  };

  const menuItems = [
    { id: 'home', label: 'Beranda', lines: ['Beranda'] },
    { id: 'about', label: 'Tentang RE', lines: ['Tentang', 'RE'] },
    { id: 'rolemodel', label: 'Role Model', lines: ['Role', 'Model'] },
    { id: 'healthcheck', label: 'Health Check', lines: ['Health', 'Check'] },
    { id: 'password', label: 'Password Checker', lines: ['Password', 'Checker'] },
    { id: 'simulation', label: 'Simulasi', lines: ['Simulasi'] },
    { id: 'chaperone', label: 'Tanya Chaperone ✨', lines: ['Tanya', 'Chaperone ✨'], isAi: true },
    { id: 'dashboard', label: 'Dashboard', lines: ['Dashboard'] },
    { id: 'profile', label: 'Profil', lines: ['Profil'] }
  ];

  return (
    <div className={`min-h-screen font-sans flex flex-col transition-colors duration-300 ${darkMode ? 'bg-gray-950 text-gray-100' : 'bg-gray-50 text-gray-800'}`}>
      
      {/* Top Header Navigation (Persis Seperti Screenshot 2026-06-23 005528.png) */}
      <header className="sticky top-0 z-50 w-full bg-white dark:bg-gray-900 border-b border-gray-200 dark:border-gray-800 shadow-sm transition-colors duration-300">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-20 flex items-center justify-between">
          
          {/* Brand Logo & Icon (Left Side) */}
          <div className="flex items-center gap-3 cursor-pointer" onClick={() => navigateTo('home')}>
            <div className="relative flex items-center justify-center w-11 h-11 rounded-full bg-gradient-to-br from-teal-500 to-blue-600 shadow-md">
              <Shield className="text-white" size={20} />
              <div className="absolute -top-0.5 -right-0.5 bg-blue-500 text-white rounded-full p-0.5 border border-white dark:border-gray-900">
                <Sparkles size={8} className="animate-pulse" />
              </div>
            </div>
            
            <div className="flex flex-col leading-none font-sans">
              <span className="text-teal-600 dark:text-teal-400 font-black text-sm tracking-tight">RE-</span>
              <span className="text-blue-600 dark:text-blue-400 font-black text-sm tracking-tight">Security</span>
              <span className="text-[10px] text-gray-400 dark:text-gray-500 font-bold tracking-wider mt-0.5">System</span>
            </div>
          </div>

          {/* Desktop & Tablet Navigation Links (Middle - Berjejer di Bagian Atas) */}
          <nav className="hidden md:flex items-center gap-1 xl:gap-2">
            {menuItems.map((item) => {
              const isActive = activeTab === item.id;
              return (
                <button
                  key={item.id}
                  onClick={() => navigateTo(item.id)}
                  className={`px-3 py-2 rounded-2xl text-[11px] xl:text-xs font-bold transition-all duration-200 whitespace-nowrap flex items-center justify-center h-12 ${
                    isActive
                      ? 'bg-teal-700 text-white shadow-md'
                      : 'text-slate-500 dark:text-gray-300 hover:text-teal-600 dark:hover:text-teal-400 hover:bg-teal-50/50 dark:hover:bg-gray-800'
                  } ${item.isAi ? 'border border-teal-200 dark:border-teal-800 bg-teal-50/30 dark:bg-teal-950/20 text-teal-600 dark:text-teal-300' : ''}`}
                >
                  <div className="flex flex-col items-center justify-center text-center leading-tight">
                    {item.lines.map((line, lIdx) => (
                      <span key={lIdx}>{line}</span>
                    ))}
                  </div>
                </button>
              );
            })}
          </nav>

          {/* Theme Switcher & Main CTA Button (Right Side) */}
          <div className="hidden md:flex items-center gap-3 xl:gap-4">
            <button 
              onClick={toggleDarkMode} 
              className="p-2 text-slate-600 dark:text-gray-300 hover:bg-slate-100 dark:hover:bg-gray-800 rounded-full transition-colors"
              title="Ganti Tema"
            >
              {darkMode ? <Sun size={20} /> : <Moon size={20} />}
            </button>
            
            <button
              onClick={() => navigateTo('healthcheck')}
              className="px-5 py-2.5 bg-teal-700 hover:bg-teal-600 text-white font-bold rounded-2xl shadow-md transition-all text-xs xl:text-sm whitespace-nowrap"
            >
              Mulai Evaluasi
            </button>
          </div>

          {/* Hamburger Menu & Theme (Mobile View Only) */}
          <div className="flex md:hidden items-center gap-3">
            <button 
              onClick={toggleDarkMode} 
              className="p-2 text-slate-600 dark:text-gray-300 hover:bg-slate-100 dark:hover:bg-gray-800 rounded-full transition-colors"
            >
              {darkMode ? <Sun size={20} /> : <Moon size={20} />}
            </button>
            <button 
              onClick={() => setIsMobileMenuOpen(!isMobileMenuOpen)}
              className="p-2 text-slate-600 dark:text-gray-300 hover:bg-slate-100 dark:hover:bg-gray-800 rounded-xl transition-colors"
            >
              {isMobileMenuOpen ? <X size={24} /> : <Menu size={24} />}
            </button>
          </div>

        </div>

        {/* Mobile Navigation Dropdown Menu (Mobile View Only) */}
        {isMobileMenuOpen && (
          <div className="md:hidden border-t border-gray-200 dark:border-gray-800 bg-white dark:bg-gray-900 px-4 pt-2 pb-6 space-y-1.5 shadow-lg animate-fade-in transition-colors">
            {menuItems.map((item) => (
              <button
                key={item.id}
                onClick={() => navigateTo(item.id)}
                className={`w-full text-left px-4 py-3 rounded-xl text-sm font-bold transition-all ${
                  activeTab === item.id
                    ? 'bg-teal-700 text-white'
                    : 'text-slate-600 dark:text-gray-300 hover:bg-slate-50 dark:hover:bg-gray-800'
                } flex items-center justify-between`}
              >
                <span>{item.label}</span>
                {item.isAi && <Sparkles size={14} className="text-yellow-500" />}
              </button>
            ))}
            <div className="pt-4 border-t border-gray-100 dark:border-gray-800">
              <button
                onClick={() => navigateTo('healthcheck')}
                className="w-full py-3 bg-teal-700 hover:bg-teal-600 text-white font-bold rounded-xl shadow-md text-center transition-all block"
              >
                Mulai Evaluasi
              </button>
            </div>
          </div>
        )}
      </header>

      {/* Main Content Area */}
      <main className="flex-grow p-4 lg:p-8 w-full bg-transparent scroll-smooth print-container">
        <div className="max-w-6xl mx-auto pb-20">
          {activeTab === 'home' && <HomeView navigateTo={navigateTo} darkMode={darkMode} />}
          {activeTab === 'about' && <AboutREView darkMode={darkMode} />}
          {activeTab === 'rolemodel' && <RoleModelView darkMode={darkMode} />}
          {activeTab === 'healthcheck' && <HealthCheckView darkMode={darkMode} />}
          {activeTab === 'password' && <PasswordCheckerView darkMode={darkMode} />}
          {activeTab === 'simulation' && <SimulationView darkMode={darkMode} />}
          {activeTab === 'chaperone' && <ChaperoneView darkMode={darkMode} />}
          {activeTab === 'dashboard' && <DashboardView darkMode={darkMode} />}
          {activeTab === 'profile' && <ProfileView darkMode={darkMode} />}
        </div>
      </main>

      {/* Footer */}
      <footer className="py-6 border-t border-gray-200 dark:border-gray-800 text-center text-xs text-slate-400 dark:text-gray-500 bg-white dark:bg-gray-900 transition-colors">
        &copy; 2026 Kelompok 7 Biologi • Program Studi Pendidikan Biologi • Universitas Lampung
      </footer>
      
      {/* Print styles */}
      <style dangerouslySetInnerHTML={{__html: `
        @media print {
          body * { visibility: hidden; }
          .print-container, .print-container * { visibility: visible; }
          .print-container { position: absolute; left: 0; top: 0; width: 100%; }
          header, button:not(.print-btn), footer { display: none !important; }
        }
      `}} />
    </div>
  );
};

/* --- 1. HOME VIEW --- */
const HomeView = ({ navigateTo, darkMode }) => (
  <div className="animate-fade-in">
    <div className={`relative overflow-hidden rounded-3xl ${darkMode ? 'bg-gray-800' : 'bg-white'} shadow-2xl`}>
      <div className="absolute top-0 right-0 -mr-20 -mt-20 w-96 h-96 rounded-full bg-teal-500 opacity-10 blur-3xl"></div>
      <div className="absolute bottom-0 left-0 -ml-20 -mb-20 w-80 h-80 rounded-full bg-blue-500 opacity-10 blur-3xl"></div>
      
      <div className="grid lg:grid-cols-2 gap-8 p-8 lg:p-12 items-center relative z-10">
        <div className="space-y-6 text-center lg:text-left">
          <div className="inline-flex items-center gap-2 px-4 py-2 rounded-full bg-teal-100 text-teal-800 dark:bg-teal-900 dark:text-teal-200 text-sm font-semibold mb-2">
            <Activity size={16} /> Inovasi Edukasi Cybersecurity
          </div>
          <h1 className="text-4xl lg:text-6xl font-extrabold tracking-tight">
            <span className="text-transparent bg-clip-text bg-gradient-to-r from-teal-500 to-blue-600">RE-Security</span> System
          </h1>
          <p className="text-xl lg:text-2xl font-medium text-gray-600 dark:text-gray-300">
            Inspirasi Keamanan Digital dari Mekanisme Retikulum Endoplasma
          </p>
          <p className="text-gray-500 dark:text-gray-400 leading-relaxed">
            Aplikasi edukatif interaktif untuk mengevaluasi keamanan akun digital mahasiswa berdasarkan sistem kontrol kualitas dan pertahanan sel biologis. Mari pelajari bagaimana sel melindungi dirinya, dan terapkan pada data digitalmu!
          </p>
          <div className="flex flex-col sm:flex-row gap-4 justify-center lg:justify-start pt-4">
            <button 
              onClick={() => navigateTo('healthcheck')}
              className="px-8 py-4 bg-teal-600 hover:bg-teal-500 text-white font-bold rounded-xl shadow-lg hover:shadow-teal-500/30 transition-all flex items-center justify-center gap-2"
            >
              <ShieldCheck size={20} /> Mulai Evaluasi
            </button>
            <button 
              onClick={() => navigateTo('about')}
              className={`px-8 py-4 font-bold rounded-xl border-2 transition-all flex items-center justify-center gap-2 ${darkMode ? 'border-gray-600 text-gray-300 hover:bg-gray-800' : 'border-teal-200 text-teal-700 hover:bg-teal-50'}`}
            >
              <Database size={20} /> Pelajari Retikulum Endoplasma
            </button>
          </div>
        </div>
        
        <div className="relative flex justify-center items-center p-4">
          <div className="relative w-full max-w-md aspect-square">
            <div className="absolute inset-0 bg-blue-100 dark:bg-blue-900/30 rounded-[40%_60%_70%_30%/40%_50%_60%_50%] animate-[spin_20s_linear_infinite] opacity-50"></div>
            <div className="absolute inset-4 bg-teal-100 dark:bg-teal-900/40 rounded-[60%_40%_30%_70%/60%_30%_70%_40%] animate-[spin_25s_linear_infinite_reverse] opacity-50"></div>
            
            <div className={`absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-32 h-32 rounded-full ${darkMode ? 'bg-gray-800 border-gray-700' : 'bg-white border-gray-100'} border-4 shadow-xl flex items-center justify-center z-20`}>
              <Lock className="text-teal-500" size={48} />
            </div>
            
            <svg viewBox="0 0 200 200" className="absolute inset-0 w-full h-full z-10 animate-pulse">
              <path d="M 40 100 Q 60 40 100 40 Q 140 40 160 100 Q 140 160 100 160 Q 60 160 40 100 Z" fill="none" stroke="currentColor" strokeWidth="2" className="text-teal-400 dark:text-teal-600" strokeDasharray="5,5" />
              <path d="M 20 100 C 20 50, 60 20, 100 20 C 140 20, 180 50, 180 100 C 180 150, 140 180, 100 180 C 60 180, 20 150, 20 100 Z" fill="none" stroke="currentColor" strokeWidth="4" className="text-blue-400 dark:text-blue-600" />
            </svg>
            
            <div className="absolute top-1/4 left-1/4 bg-white dark:bg-gray-800 p-2 rounded-full shadow-lg z-20"><ShieldCheck className="text-green-500" size={24}/></div>
            <div className="absolute bottom-1/4 right-1/4 bg-white dark:bg-gray-800 p-2 rounded-full shadow-lg z-20"><AlertTriangle className="text-yellow-500" size={24}/></div>
          </div>
        </div>
      </div>
    </div>
  </div>
);

/* --- 2. ABOUT RE VIEW --- */
const AboutREView = ({ darkMode }) => (
  <div className="animate-fade-in space-y-8">
    <div className="text-center space-y-4">
      <h2 className="text-3xl lg:text-4xl font-bold">Mengenal <span className="text-teal-500">Retikulum Endoplasma</span></h2>
      <p className="text-lg max-w-3xl mx-auto text-gray-600 dark:text-gray-400">
        Retikulum Endoplasma (RE) adalah organel vital dalam sel eukariotik yang bertindak sebagai pabrik utama untuk sintesis, pelipatan, dan modifikasi protein serta lipid, sekaligus sebagai sistem keamanan internal sel.
      </p>
    </div>

    <div className="grid md:grid-cols-2 gap-8">
      <div className={`p-6 rounded-2xl shadow-lg ${darkMode ? 'bg-gray-800' : 'bg-white'}`}>
        <h3 className="text-2xl font-bold mb-6 flex items-center gap-2 border-b pb-4 border-teal-100 dark:border-teal-900">
          <Database className="text-blue-500" /> Struktur RE
        </h3>
        <div className="space-y-4">
          <div className="flex gap-4">
            <div className="flex-shrink-0 w-12 h-12 bg-blue-100 dark:bg-blue-900 rounded-full flex items-center justify-center font-bold text-blue-600 dark:text-blue-300">1</div>
            <div>
              <h4 className="font-bold text-lg">RE Kasar (RER)</h4>
              <p className="text-sm text-gray-600 dark:text-gray-400">Permukaannya ditempeli ribosom. Tempat utama sintesis protein yang akan disekresikan atau disisipkan ke membran.</p>
            </div>
          </div>
          <div className="flex gap-4">
            <div className="flex-shrink-0 w-12 h-12 bg-blue-100 dark:bg-blue-900 rounded-full flex items-center justify-center font-bold text-blue-600 dark:text-blue-300">2</div>
            <div>
              <h4 className="font-bold text-lg">RE Halus (SER)</h4>
              <p className="text-sm text-gray-600 dark:text-gray-400">Tanpa ribosom. Berperan dalam sintesis lipid, metabolisme karbohidrat, dan detoksifikasi racun.</p>
            </div>
          </div>
          <div className="flex gap-4">
            <div className="flex-shrink-0 w-12 h-12 bg-blue-100 dark:bg-blue-900 rounded-full flex items-center justify-center font-bold text-blue-600 dark:text-blue-300">3</div>
            <div>
              <h4 className="font-bold text-lg">Lumen</h4>
              <p className="text-sm text-gray-600 dark:text-gray-400">Ruang di dalam kantung RE tempat protein melipat dan dimodifikasi menjadi bentuk fungsionalnya.</p>
            </div>
          </div>
          <div className="flex gap-4">
            <div className="flex-shrink-0 w-12 h-12 bg-blue-100 dark:bg-blue-900 rounded-full flex items-center justify-center font-bold text-blue-600 dark:text-blue-300">4</div>
            <div>
              <h4 className="font-bold text-lg">Membran RE</h4>
              <p className="text-sm text-gray-600 dark:text-gray-400">Lapisan ganda fosfolipid yang memisahkan lumen dari sitosol, mengontrol apa yang masuk dan keluar.</p>
            </div>
          </div>
        </div>
      </div>

      <div className={`p-6 rounded-2xl shadow-lg ${darkMode ? 'bg-gray-800' : 'bg-white'}`}>
        <h3 className="text-2xl font-bold mb-6 flex items-center gap-2 border-b pb-4 border-teal-100 dark:border-teal-900">
          <Activity className="text-teal-500" /> Fungsi Utama
        </h3>
        <div className="grid grid-cols-1 gap-4">
          <div className={`p-4 rounded-xl border ${darkMode ? 'border-gray-700 bg-gray-750' : 'border-teal-100 bg-teal-50'}`}>
            <h4 className="font-bold text-teal-700 dark:text-teal-300 flex items-center gap-2 mb-1"><Settings size={18}/> Sintesis Protein & Lipid</h4>
            <p className="text-sm text-gray-600 dark:text-gray-400">Memproduksi bahan baku utama untuk fungsi, perbaikan, dan pertumbuhan sel.</p>
          </div>
          <div className={`p-4 rounded-xl border ${darkMode ? 'border-gray-700 bg-gray-750' : 'border-teal-100 bg-teal-50'}`}>
            <h4 className="font-bold text-teal-700 dark:text-teal-300 flex items-center gap-2 mb-1"><Shield size={18}/> Detoksifikasi</h4>
            <p className="text-sm text-gray-600 dark:text-gray-400">RE Halus (terutama di hati) memecah racun dan obat-obatan agar tidak membahayakan tubuh.</p>
          </div>
          <div className={`p-4 rounded-xl border ${darkMode ? 'border-gray-700 bg-gray-750' : 'border-teal-100 bg-teal-50'}`}>
            <h4 className="font-bold text-teal-700 dark:text-teal-300 flex items-center gap-2 mb-1"><CheckCircle size={18}/> Quality Control Protein</h4>
            <p className="text-sm text-gray-600 dark:text-gray-400">Sistem keamanan ketat yang memastikan hanya protein dengan pelipatan sempurna yang boleh keluar dari RE.</p>
          </div>
        </div>
      </div>
    </div>
  </div>
);

/* --- 3. ROLE MODEL ACTION VIEW --- */
const RoleModelView = ({ darkMode }) => {
  const [activeIndex, setActiveIndex] = useState(null);

  const mappings = [
    {
      re: "Membran RE",
      cyber: "Firewall",
      reIcon: <Database />, cyberIcon: <ShieldCheck />,
      desc: "Sama seperti Membran RE yang menjadi lapisan pelindung fisik sel, Firewall adalah sistem keamanan jaringan yang memantau dan mengontrol lalu lintas yang masuk dan keluar berdasarkan aturan keamanan."
    },
    {
      re: "Translokon",
      cyber: "Login + 2FA",
      reIcon: <Unlock />, cyberIcon: <Smartphone />,
      desc: "Translokon adalah gerbang khusus bagi protein untuk masuk ke RE. Di dunia digital, halaman Login dan 2FA (Two-Factor Authentication) adalah gerbang ketat untuk memverifikasi identitas pengguna."
    },
    {
      re: "Chaperone",
      cyber: "Sistem Deteksi Ancaman",
      reIcon: <CheckCircle />, cyberIcon: <Activity />,
      desc: "Protein Chaperone membantu melipat protein dan mendeteksi kesalahan (misfold). Dalam cyber, IDS/IPS mendeteksi perilaku mencurigakan atau kode berbahaya sebelum merusak sistem."
    },
    {
      re: "ERAD (ER-Associated Degradation)",
      cyber: "Antivirus / Karantina",
      reIcon: <XCircle />, cyberIcon: <ShieldAlert />,
      desc: "ERAD menghancurkan protein yang gagal melipat atau berbahaya. Antivirus menghapus atau mengkarantina malware dan file korup agar tidak menginfeksi seluruh sistem."
    },
    {
      re: "Lumen RE",
      cyber: "Enkripsi Data",
      reIcon: <Lock />, cyberIcon: <Key />,
      desc: "Lumen menyediakan lingkungan aman untuk pematangan protein. Enkripsi mengamankan data yang tersimpan (Data at Rest) sehingga tidak bisa dibaca jika diretas."
    }
  ];

  return (
    <div className="animate-fade-in space-y-6">
      <div className="text-center mb-8">
        <h2 className="text-3xl font-bold mb-2">Role Model Action</h2>
        <p className="text-gray-600 dark:text-gray-400">Klik pada komponen untuk melihat analogi mendalam antara Biologi dan Cybersecurity.</p>
      </div>

      <div className="grid grid-cols-2 md:grid-cols-5 gap-4 mb-8">
        {mappings.map((item, index) => (
          <button
            key={index}
            onClick={() => setActiveIndex(activeIndex === index ? null : index)}
            className={`p-4 rounded-2xl flex flex-col items-center text-center transition-all duration-300 border-2 ${
              activeIndex === index 
                ? 'border-teal-500 bg-teal-50 dark:bg-teal-900/30 scale-105 shadow-lg' 
                : `${darkMode ? 'bg-gray-800 border-gray-700 hover:border-teal-600' : 'bg-white border-gray-200 hover:border-teal-300'}`
            }`}
          >
            <div className="flex w-full justify-between mb-4">
              <div className="text-blue-500">{item.reIcon}</div>
              <div className="text-teal-500">{item.cyberIcon}</div>
            </div>
            <span className="font-bold text-sm mb-1">{item.re}</span>
            <div className="h-px w-8 bg-gray-300 my-2"></div>
            <span className="font-semibold text-xs text-teal-600 dark:text-teal-400">{item.cyber}</span>
          </button>
        ))}
      </div>

      {activeIndex !== null && (
        <div className={`p-6 rounded-2xl animate-fade-in shadow-xl border-l-4 border-teal-500 ${darkMode ? 'bg-gray-800' : 'bg-white'}`}>
          <div className="flex items-center gap-4 mb-4 border-b border-gray-200 dark:border-gray-700 pb-4">
            <h3 className="text-xl md:text-2xl font-bold flex flex-wrap items-center gap-2">
              <span className="text-blue-500">{mappings[activeIndex].re}</span> 
              <span className="text-gray-400 text-sm">diibaratkan sebagai</span> 
              <span className="text-teal-500">{mappings[activeIndex].cyber}</span>
            </h3>
          </div>
          <p className="text-base md:text-lg leading-relaxed text-gray-700 dark:text-gray-300">
            {mappings[activeIndex].desc}
          </p>
        </div>
      )}
    </div>
  );
};

/* --- 4. SECURITY HEALTH CHECK VIEW --- */
const HealthCheckView = ({ darkMode }) => {
  const [answers, setAnswers] = useState({});
  const [showResult, setShowResult] = useState(false);
  const [score, setScore] = useState(0);
  const [aiAdvice, setAiAdvice] = useState("");
  const [isAiLoading, setIsAiLoading] = useState(false);

  const questions = [
    { id: 1, text: "Apakah Anda menggunakan password yang berbeda untuk setiap akun digital Anda?", options: [{ label: "Ya, selalu berbeda", val: 20 }, { label: "Hanya untuk akun penting", val: 10 }, { label: "Tidak, saya menggunakan 1-2 password yang sama", val: 0 }] },
    { id: 2, text: "Apakah Anda mengaktifkan 2FA (Two-Factor Authentication) di email dan media sosial?", options: [{ label: "Ya, semuanya", val: 20 }, { label: "Hanya di akun bank/keuangan", val: 10 }, { label: "Tidak/Belum", val: 0 }] },
    { id: 3, text: "Seberapa sering Anda mengganti password?", options: [{ label: "< 3 Bulan", val: 20 }, { label: "3 - 6 Bulan", val: 15 }, { label: "> 6 Bulan / Tidak pernah", val: 0 }] },
    { id: 4, text: "Pernahkah Anda mengalami peretasan akun?", options: [{ label: "Tidak Pernah", val: 20 }, { label: "Pernah 1 kali, tapi sudah aman", val: 10 }, { label: "Sering/Pernah diretas lebih dari 1 kali", val: 0 }] },
    { id: 5, text: "Pernahkah Anda mengklik link mencurigakan atau menjadi korban phishing?", options: [{ label: "Tidak pernah", val: 10 }, { label: "Pernah", val: 0 }] },
    { id: 6, text: "Apakah perangkat (HP/Laptop) Anda memiliki sistem keamanan atau antivirus yang aktif?", options: [{ label: "Ya, aktif dan update", val: 10 }, { label: "Tidak ada/Kadang dinonaktifkan", val: 0 }] }
  ];

  const handleSelect = (qId, val) => {
    setAnswers(prev => ({ ...prev, [qId]: val }));
  };

  const calculateScore = () => {
    const total = Object.values(answers).reduce((acc, curr) => acc + curr, 0);
    setScore(total);
    setShowResult(true);
  };

  const resetQuiz = () => {
    setAnswers({});
    setShowResult(false);
    setScore(0);
    setAiAdvice("");
  };

  const getAiAdvice = async () => {
    setIsAiLoading(true);
    setAiAdvice("");
    const prompt = `Sebagai pakar cybersecurity dan biologi, berikan 2 paragraf singkat saran keamanan digital (dengan menggunakan analogi organel sel Retikulum Endoplasma) untuk pengguna yang mendapat skor ${score}/100 pada evaluasi keamanan akun mereka. Berikan nada yang edukatif dan menyemangati.`;
    try {
      const text = await callGeminiAPI(prompt);
      setAiAdvice(text);
    } catch(e) {
      setAiAdvice(e.message);
    }
    setIsAiLoading(false);
  };

  const allAnswered = Object.keys(answers).length === questions.length;

  let resultData = { title: "", desc: "", color: "", bg: "", icon: null };
  if (score >= 90) {
    resultData = { title: "Sel Aman - Sistem RE Berfungsi Optimal", desc: "Pertahanan digital Anda sangat kuat, layaknya Retikulum Endoplasma yang sehat. Terus pertahankan kontrol kualitas Anda!", color: "text-green-500", bg: "bg-green-100 dark:bg-green-900/30", icon: <ShieldCheck size={48} className="text-green-500"/> };
  } else if (score >= 70) {
    resultData = { title: "Sel Stabil - Perlu Sedikit Perbaikan", desc: "Keamanan Anda cukup baik, namun ada celah kecil. Aktifkan 2FA di semua akun dan mulai gunakan password manager layaknya Chaperone RE.", color: "text-blue-500", bg: "bg-blue-100 dark:bg-blue-900/30", icon: <Shield size={48} className="text-blue-500"/> };
  } else if (score >= 50) {
    resultData = { title: "Stres RE Terdeteksi", desc: "Sistem Anda mulai kewalahan (ER Stress). Risiko peretasan cukup tinggi. Segera ganti password yang sama dan aktifkan 2FA!", color: "text-yellow-500", bg: "bg-yellow-100 dark:bg-yellow-900/30", icon: <AlertTriangle size={48} className="text-yellow-500"/> };
  } else {
    resultData = { title: "Risiko Tinggi Peretasan (Apoptosis/Kematian Sel)", desc: "Sistem keamanan Anda sangat rentan! Ibarat sel yang rusak parah. Anda harus segera mengubah semua password dan memasang antivirus sebelum terlambat.", color: "text-red-500", bg: "bg-red-100 dark:bg-red-900/30", icon: <ShieldAlert size={48} className="text-red-500"/> };
  }

  return (
    <div className="animate-fade-in max-w-3xl mx-auto space-y-6 print-container">
      <div className="text-center mb-8">
        <h2 className="text-3xl font-bold">Security Health Check</h2>
        <p className="text-gray-600 dark:text-gray-400">Evaluasi kesehatan sistem keamanan digital Anda.</p>
      </div>

      {!showResult ? (
        <div className={`p-6 rounded-2xl shadow-lg ${darkMode ? 'bg-gray-800' : 'bg-white'}`}>
          <div className="space-y-8">
            {questions.map((q, i) => (
              <div key={q.id} className="space-y-3">
                <p className="font-bold text-lg"><span className="text-teal-500 mr-2">{i+1}.</span> {q.text}</p>
                <div className="grid sm:grid-cols-2 gap-3 pl-6">
                  {q.options.map((opt, j) => (
                    <button
                      key={j}
                      onClick={() => handleSelect(q.id, opt.val)}
                      className={`text-left p-3 rounded-xl border-2 transition-all ${answers[q.id] === opt.val ? 'border-teal-500 bg-teal-50 text-teal-800 dark:bg-teal-900/40 dark:text-teal-200 font-bold' : `${darkMode ? 'border-gray-700 hover:border-gray-500' : 'border-gray-200 hover:border-gray-300'}`}`}
                    >
                      {opt.label}
                    </button>
                  ))}
                </div>
              </div>
            ))}
          </div>

          <div className="mt-8 pt-6 border-t border-gray-200 dark:border-gray-700 flex justify-end">
            <button
              disabled={!allAnswered}
              onClick={calculateScore}
              className={`px-8 py-3 rounded-xl font-bold text-white transition-all ${allAnswered ? 'bg-teal-600 hover:bg-teal-500 shadow-lg' : 'bg-gray-400 cursor-not-allowed'}`}
            >
              Lihat Hasil Diagnosa
            </button>
          </div>
        </div>
      ) : (
        <div className={`p-8 rounded-3xl shadow-xl text-center space-y-6 border-t-8 ${score >= 70 ? 'border-green-500' : score >= 50 ? 'border-yellow-500' : 'border-red-500'} ${darkMode ? 'bg-gray-800' : 'bg-white'}`}>
          <div className="flex justify-center mb-4 animate-bounce">
            {resultData.icon}
          </div>
          <h3 className="text-xl font-medium text-gray-500 dark:text-gray-400">Skor Kesehatan Sistem Anda:</h3>
          <div className={`text-6xl font-black ${resultData.color}`}>{score} / 100</div>
          
          <div className={`p-6 rounded-2xl ${resultData.bg} mx-auto max-w-lg mt-6`}>
            <h4 className={`text-2xl font-bold mb-2 ${resultData.color}`}>{resultData.title}</h4>
            <p className="text-lg dark:text-gray-200">{resultData.desc}</p>
          </div>

          {/* AI Advice Integration */}
          <div className="mt-6">
            {!aiAdvice && !isAiLoading && (
              <button onClick={getAiAdvice} className="px-6 py-3 rounded-xl bg-purple-600 hover:bg-purple-500 text-white font-bold shadow-lg transition-all flex items-center gap-2 mx-auto">
                <Sparkles size={20} /> Analisis Mendalam ✨
              </button>
            )}
            {isAiLoading && (
              <div className="flex items-center justify-center gap-2 text-purple-500 font-bold">
                <Loader2 size={24} className="animate-spin" /> Chaperone sedang menganalisis profil keamanan Anda...
              </div>
            )}
            {aiAdvice && (
              <div className={`p-6 mt-4 rounded-2xl text-left border border-purple-300 shadow-inner ${darkMode ? 'bg-purple-900/20 text-gray-200' : 'bg-purple-50 text-gray-800'}`}>
                <h4 className="font-bold flex items-center gap-2 text-purple-600 dark:text-purple-400 mb-3">
                  <Sparkles size={20} /> Saran Personal dari AI Chaperone:
                </h4>
                <div className="space-y-3 whitespace-pre-wrap leading-relaxed text-sm md:text-base">
                  {aiAdvice}
                </div>
              </div>
            )}
          </div>

          <div className="flex justify-center gap-4 pt-8 print:hidden">
            <button onClick={resetQuiz} className="px-6 py-2 rounded-lg border-2 border-teal-500 text-teal-600 dark:text-teal-400 font-bold hover:bg-teal-50 dark:hover:bg-teal-900/30 transition-all">
              Ulangi Evaluasi
            </button>
            <button onClick={() => window.print()} className="print-btn px-6 py-2 rounded-lg bg-teal-600 text-white font-bold hover:bg-teal-500 shadow-lg transition-all flex items-center gap-2">
              <Download size={18} /> Unduh Hasil (PDF)
            </button>
          </div>
        </div>
      )}
    </div>
  );
};

/* --- 5. PASSWORD CHECKER VIEW --- */
const PasswordCheckerView = ({ darkMode }) => {
  const [pwd, setPwd] = useState("");

  const analyzePassword = (password) => {
    let score = 0;
    let criteria = { length: false, upper: false, lower: false, num: false, sym: false };

    if (password.length >= 8) { score += 20; criteria.length = true; }
    if (/[A-Z]/.test(password)) { score += 20; criteria.upper = true; }
    if (/[a-z]/.test(password)) { score += 20; criteria.lower = true; }
    if (/[0-9]/.test(password)) { score += 20; criteria.num = true; }
    if (/[^A-Za-z0-9]/.test(password)) { score += 20; criteria.sym = true; }

    return { score, criteria };
  };

  const { score, criteria } = analyzePassword(pwd);

  let status = { text: "Menunggu Input", color: "text-gray-400", bar: "bg-gray-200", analogy: "Protein belum disintesis." };
  if (pwd.length > 0) {
    if (score <= 40) { status = { text: "Lemah", color: "text-red-500", bar: "bg-red-500", analogy: "Ditolak oleh Chaperone RE (Protein Misfold/Rusak). Segera degradasi!" }; }
    else if (score <= 80) { status = { text: "Sedang", color: "text-yellow-500", bar: "bg-yellow-500", analogy: "Tertahan di Lumen RE. Masih perlu modifikasi pelipatan agar sempurna." }; }
    else { status = { text: "Kuat", color: "text-green-500", bar: "bg-green-500", analogy: "Lolos Quality Control RE! Protein sempurna siap dikirim ke Golgi." }; }
  }

  return (
    <div className="animate-fade-in max-w-2xl mx-auto space-y-6">
      <div className="text-center mb-8">
        <h2 className="text-3xl font-bold mb-2">Password Checker & Quality Control</h2>
        <p className="text-gray-600 dark:text-gray-400">Uji kekuatan password Anda layaknya RE menyeleksi protein.</p>
      </div>

      <div className={`p-8 rounded-3xl shadow-xl ${darkMode ? 'bg-gray-800' : 'bg-white'}`}>
        <div className="relative mb-8">
          <Key className="absolute top-4 left-4 text-teal-500" size={24} />
          <input
            type="text"
            placeholder="Ketik password untuk diuji..."
            value={pwd}
            onChange={(e) => setPwd(e.target.value)}
            className={`w-full p-4 pl-12 rounded-xl text-lg border-2 outline-none focus:border-teal-500 transition-colors ${darkMode ? 'bg-gray-900 border-gray-700 text-white' : 'bg-gray-50 border-gray-200'}`}
          />
        </div>

        <div className="mb-6">
          <div className="flex justify-between mb-2">
            <span className="font-bold text-sm">Kekuatan: <span className={status.color}>{status.text}</span></span>
            <span className="font-bold text-sm">{score} / 100</span>
          </div>
          <div className="h-4 w-full bg-gray-200 dark:bg-gray-700 rounded-full overflow-hidden">
            <div className={`h-full ${status.bar} transition-all duration-500 ease-out`} style={{ width: `${score}%` }}></div>
          </div>
        </div>

        <div className={`p-4 rounded-xl mb-6 flex items-start gap-3 border ${darkMode ? 'border-gray-700 bg-gray-750' : 'border-teal-100 bg-teal-50'}`}>
          <div className="mt-1">{score === 100 ? <CheckCircle className="text-green-500"/> : <Activity className="text-blue-500"/>}</div>
          <div>
            <p className="text-xs text-gray-500 dark:text-gray-400 font-bold uppercase tracking-wider mb-1">Status Quality Control RE</p>
            <p className="text-sm font-medium">{status.analogy}</p>
          </div>
        </div>

        <div className="grid grid-cols-2 gap-3 text-sm">
          <div className={`flex items-center gap-2 ${criteria.length ? 'text-green-500' : 'text-gray-400'}`}>
            {criteria.length ? <CheckCircle size={16}/> : <XCircle size={16}/>} Minimal 8 Karakter
          </div>
          <div className={`flex items-center gap-2 ${criteria.upper ? 'text-green-500' : 'text-gray-400'}`}>
            {criteria.upper ? <CheckCircle size={16}/> : <XCircle size={16}/>} Huruf Besar (A-Z)
          </div>
          <div className={`flex items-center gap-2 ${criteria.lower ? 'text-green-500' : 'text-gray-400'}`}>
            {criteria.lower ? <CheckCircle size={16}/> : <XCircle size={16}/>} Huruf Kecil (a-z)
          </div>
          <div className={`flex items-center gap-2 ${criteria.num ? 'text-green-500' : 'text-gray-400'}`}>
            {criteria.num ? <CheckCircle size={16}/> : <XCircle size={16}/>} Angka (0-9)
          </div>
          <div className={`flex items-center gap-2 ${criteria.sym ? 'text-green-500' : 'text-gray-400'}`}>
            {criteria.sym ? <CheckCircle size={16}/> : <XCircle size={16}/>} Simbol (!@#$%)
          </div>
        </div>

      </div>
    </div>
  );
};

/* --- 6. SIMULATION VIEW --- */
const SimulationView = ({ darkMode }) => {
  const [step, setStep] = useState(0);
  
  const processSteps = [
    { title: "Memulai Koneksi", cyber: "Pengguna meminta akses", re: "Protein mulai disintesis oleh Ribosom", icon: <Monitor /> },
    { title: "Pemeriksaan Firewall", cyber: "Firewall menyaring IP dan Port", re: "Membran RE menyeleksi molekul masuk", icon: <ShieldCheck /> },
    { title: "Login Pengguna", cyber: "Input Username & Password", re: "Protein masuk melalui Translokon", icon: <Unlock /> },
    { title: "Verifikasi OTP (2FA)", cyber: "Sistem meminta kode tambahan", re: "Pengecekan sinyal peptida spesifik", icon: <Smartphone /> },
    { title: "Deteksi Ancaman", cyber: "IDS/IPS memindai anomali/malware", re: "Protein Chaperone memeriksa kesalahan pelipatan", icon: <Activity /> },
    { title: "Karantina / Enkripsi", cyber: "Data aman dienkripsi, malware dibuang", re: "ERAD menghancurkan misfold, Lumen mengamankan protein", icon: <Lock /> },
    { title: "Akses Diberikan", cyber: "Login Berhasil, akses ke database", re: "Protein sukses dilipat dan siap dikirim ke Golgi", icon: <CheckCircle /> }
  ];

  const nextStep = () => {
    if (step < processSteps.length - 1) setStep(step + 1);
  };
  const resetSim = () => setStep(0);

  return (
    <div className="animate-fade-in space-y-6 max-w-4xl mx-auto">
      <div className="text-center mb-8">
        <h2 className="text-3xl font-bold mb-2">Simulasi RE-Security System</h2>
        <p className="text-gray-600 dark:text-gray-400">Amati bagaimana sistem keamanan berlapis bekerja mengamankan data Anda.</p>
      </div>

      <div className={`p-8 rounded-3xl shadow-xl ${darkMode ? 'bg-gray-800' : 'bg-white'}`}>
        
        <div className="relative h-64 bg-gray-900 rounded-2xl mb-8 overflow-hidden flex items-center justify-center border-4 border-gray-700">
           <div className="absolute inset-0 opacity-20" style={{ backgroundImage: 'linear-gradient(#374151 1px, transparent 1px), linear-gradient(90deg, #374151 1px, transparent 1px)', backgroundSize: '20px 20px'}}></div>
           
           <div className={`z-10 transform transition-all duration-700 flex flex-col items-center gap-2 
              ${step === 0 ? 'translate-x-[-150px] opacity-50' : ''}
              ${step === 1 ? 'translate-x-[-100px] scale-110 text-blue-400' : ''}
              ${step === 2 ? 'translate-x-[-50px] scale-100 text-yellow-400' : ''}
              ${step === 3 ? 'translate-x-[0px] scale-125 text-teal-400 animate-pulse' : ''}
              ${step === 4 ? 'translate-x-[50px] scale-100 text-red-400' : ''}
              ${step === 5 ? 'translate-x-[100px] scale-100 text-purple-400' : ''}
              ${step === 6 ? 'translate-x-[150px] scale-125 text-green-500 drop-shadow-[0_0_15px_rgba(34,197,94,0.8)]' : ''}
           `}>
             <div className="p-4 bg-gray-800 rounded-full border-2 border-current shadow-lg text-white">
                {processSteps[step].icon}
             </div>
             <span className="text-white font-bold tracking-widest uppercase text-xs">Data / Protein</span>
           </div>

           <div className={`absolute left-1/4 top-0 bottom-0 w-1 ${step > 0 ? 'bg-green-500/50' : 'bg-red-500/50'} transition-colors duration-500`}></div>
           <div className={`absolute left-2/4 top-0 bottom-0 w-1 ${step > 2 ? 'bg-green-500/50' : 'bg-red-500/50'} transition-colors duration-500`}></div>
           <div className={`absolute left-3/4 top-0 bottom-0 w-1 ${step > 4 ? 'bg-green-500/50' : 'bg-red-500/50'} transition-colors duration-500`}></div>
        </div>

        <div className="grid md:grid-cols-2 gap-6 items-center">
          <div>
            <h3 className="text-xl font-bold mb-1 text-teal-600 dark:text-teal-400">Tahap {step + 1}: {processSteps[step].title}</h3>
            <div className="space-y-3 mt-4">
              <div className="flex items-start gap-2">
                <Shield className="text-blue-500 mt-1 flex-shrink-0" size={18}/>
                <p className="text-sm"><span className="font-bold">Cyber:</span> {processSteps[step].cyber}</p>
              </div>
              <div className="flex items-start gap-2">
                <Database className="text-green-500 mt-1 flex-shrink-0" size={18}/>
                <p className="text-sm"><span className="font-bold">Biologi (RE):</span> {processSteps[step].re}</p>
              </div>
            </div>
          </div>
          
          <div className="flex flex-col gap-3 md:items-end">
            {step < processSteps.length - 1 ? (
              <button onClick={nextStep} className="w-full md:w-auto px-6 py-3 bg-teal-600 hover:bg-teal-500 text-white font-bold rounded-xl shadow-lg transition-all flex items-center justify-center gap-2">
                Lanjutkan Proses <ChevronRight size={20}/>
              </button>
            ) : (
              <button onClick={resetSim} className="w-full md:w-auto px-6 py-3 bg-blue-600 hover:bg-blue-500 text-white font-bold rounded-xl shadow-lg transition-all flex items-center justify-center gap-2">
                Ulangi Simulasi
              </button>
            )}
            
            <div className="flex gap-1 mt-2">
              {processSteps.map((_, i) => (
                <div key={i} className={`h-2 w-6 rounded-full transition-all duration-300 ${i === step ? 'bg-teal-500 w-10' : i < step ? 'bg-teal-200 dark:bg-teal-900' : 'bg-gray-200 dark:bg-gray-700'}`}></div>
              ))}
            </div>
          </div>
        </div>

      </div>
    </div>
  );
};

/* --- 7. CHAPERONE AI VIEW --- */
const ChaperoneView = ({ darkMode }) => {
  const [messages, setMessages] = useState([
    { role: 'assistant', content: 'Halo! Saya adalah **Chaperone AI**. Saya bertugas memastikan "protein data" Anda melipat dengan benar dan aman dari ancaman. Ada yang ingin Anda tanyakan seputar Retikulum Endoplasma atau Keamanan Siber?' }
  ]);
  const [input, setInput] = useState("");
  const [isLoading, setIsLoading] = useState(false);

  const sendMessage = async () => {
    if (!input.trim() || isLoading) return;
    
    const userMessage = input.trim();
    setMessages(prev => [...prev, { role: 'user', content: userMessage }]);
    setInput("");
    setIsLoading(true);

    const conversation = messages.map(m => `${m.role === 'user' ? 'User' : 'Chaperone'}: ${m.content}`).join('\n');
    const systemPrompt = `Kamu adalah 'Chaperone AI', asisten edukasi di aplikasi 'RE-Security System'. Tugasmu membantu mahasiswa memahami Retikulum Endoplasma (RE) dan Keamanan Siber (Cybersecurity) menggunakan analogi seluler biologi. Jawab dengan ringkas, ramah, edukatif, dan gunakan markdown. Percakapan sebelumnya:\n${conversation}\nUser: ${userMessage}\nChaperone:`;

    try {
      const responseText = await callGeminiAPI(systemPrompt);
      setMessages(prev => [...prev, { role: 'assistant', content: responseText }]);
    } catch (error) {
      setMessages(prev => [...prev, { role: 'assistant', content: "⚠️ Maaf, terjadi gangguan pada jaringan lumen RE (Error koneksi). Coba lagi nanti." }]);
    }
    
    setIsLoading(false);
  };

  return (
    <div className="animate-fade-in max-w-4xl mx-auto space-y-6 flex flex-col h-[75vh]">
      <div className="text-center mb-4">
        <h2 className="text-3xl font-bold mb-2 flex items-center justify-center gap-2">
          <Sparkles className="text-yellow-400" size={32} /> Tanya Chaperone AI
        </h2>
        <p className="text-gray-600 dark:text-gray-400">Asisten cerdas Anda untuk belajar Biologi dan Cybersecurity.</p>
      </div>

      <div className={`flex-1 flex flex-col rounded-3xl shadow-xl overflow-hidden border ${darkMode ? 'bg-gray-800 border-gray-700' : 'bg-white border-gray-200'}`}>
        <div className="flex-1 overflow-y-auto p-6 space-y-6">
          {messages.map((msg, idx) => (
            <div key={idx} className={`flex ${msg.role === 'user' ? 'justify-end' : 'justify-start'}`}>
              <div className={`max-w-[80%] rounded-2xl p-4 ${
                msg.role === 'user' 
                  ? 'bg-teal-600 text-white rounded-br-none shadow-md' 
                  : `${darkMode ? 'bg-gray-700 text-gray-200' : 'bg-teal-50 text-gray-800'} rounded-bl-none shadow-sm border ${darkMode ? 'border-gray-600' : 'border-teal-100'}`
              }`}>
                {msg.role === 'assistant' && (
                  <div className="flex items-center gap-2 mb-2 font-bold text-teal-600 dark:text-teal-400 text-sm">
                    <CheckCircle size={16} /> Chaperone AI
                  </div>
                )}
                <div className="whitespace-pre-wrap text-sm md:text-base">{msg.content}</div>
              </div>
            </div>
          ))}
          {isLoading && (
            <div className="flex justify-start">
              <div className={`max-w-[80%] rounded-2xl p-4 rounded-bl-none shadow-sm border flex items-center gap-3 ${darkMode ? 'bg-gray-700 border-gray-600 text-gray-200' : 'bg-teal-50 border-teal-100 text-gray-800'}`}>
                 <Loader2 size={20} className="animate-spin text-teal-500" />
                 <span className="text-sm font-medium animate-pulse">Memeriksa struktur protein...</span>
              </div>
            </div>
          )}
        </div>

        <div className={`p-4 border-t ${darkMode ? 'border-gray-700 bg-gray-800' : 'border-gray-200 bg-gray-50'}`}>
          <div className="flex gap-2 relative">
            <input
              type="text"
              value={input}
              onChange={(e) => setInput(e.target.value)}
              onKeyPress={(e) => e.key === 'Enter' && sendMessage()}
              placeholder="Tanyakan sesuatu tentang analogi RE dan Cybersecurity..."
              className={`flex-1 p-4 pr-14 rounded-xl border outline-none focus:border-teal-500 transition-colors ${darkMode ? 'bg-gray-900 border-gray-700 text-white' : 'bg-white border-gray-300'}`}
              disabled={isLoading}
            />
            <button
              onClick={sendMessage}
              disabled={!input.trim() || isLoading}
              className={`absolute right-2 top-2 bottom-2 px-4 rounded-lg flex items-center justify-center transition-all ${!input.trim() || isLoading ? 'bg-gray-300 text-gray-500 cursor-not-allowed dark:bg-gray-700 dark:text-gray-500' : 'bg-teal-600 hover:bg-teal-500 text-white shadow-md'}`}
            >
              <Send size={20} />
            </button>
          </div>
        </div>

      </div>
    </div>
  );
};

/* --- 8. DASHBOARD VIEW --- */
const DashboardView = ({ darkMode }) => {
  const stats = [
    { label: "Tidak Mengganti Password Secara Rutin", val: 60, color: "bg-red-500" },
    { label: "Pernah Mengalami Peretasan Akun", val: 35, color: "bg-orange-500" },
    { label: "Mengalami Penipuan Online (Phishing)", val: 23, color: "bg-yellow-500" },
    { label: "Mengalami Kerugian Finansial Akibat Hack", val: 17, color: "bg-purple-500" },
    { label: "Data Pribadi Disalahgunakan", val: 13, color: "bg-blue-500" },
    { label: "Tidak Menggunakan 2FA", val: 13, color: "bg-teal-500" },
  ];

  return (
    <div className="animate-fade-in space-y-6 max-w-5xl mx-auto">
      <div className="text-center mb-8">
        <h2 className="text-3xl font-bold mb-2">Dashboard Real-Time Security Awareness</h2>
        <p className="text-gray-600 dark:text-gray-400">Statistik kerentanan keamanan digital mahasiswa (Berdasarkan Data Survei).</p>
      </div>

      <div className="grid lg:grid-cols-3 gap-6">
        <div className={`lg:col-span-2 p-6 rounded-3xl shadow-xl ${darkMode ? 'bg-gray-800' : 'bg-white'}`}>
          <h3 className="font-bold text-lg mb-6 border-b pb-2 dark:border-gray-700">Persentase Kerentanan</h3>
          <div className="space-y-5">
            {stats.map((stat, idx) => (
              <div key={idx}>
                <div className="flex justify-between text-sm font-medium mb-1">
                  <span>{stat.label}</span>
                  <span>{stat.val}%</span>
                </div>
                <div className="w-full bg-gray-100 dark:bg-gray-700 rounded-full h-4 overflow-hidden">
                  <div 
                    className={`h-4 rounded-full ${stat.color} hover:opacity-80 transition-all duration-1000 ease-out`}
                    style={{ width: `${stat.val}%`, animation: `slideRight 1s ease-out forwards ${idx * 0.1}s` }}
                  ></div>
                </div>
              </div>
            ))}
          </div>
        </div>

        <div className="space-y-6">
          <div className={`p-6 rounded-3xl shadow-xl text-center ${darkMode ? 'bg-teal-900/40 border border-teal-800' : 'bg-teal-50 border border-teal-100'}`}>
            <AlertTriangle className="mx-auto text-orange-500 mb-2" size={32} />
            <h4 className="font-bold text-2xl text-teal-700 dark:text-teal-300">60%</h4>
            <p className="text-sm text-gray-600 dark:text-gray-400 mt-2">Mayoritas mahasiswa masih mengabaikan pergantian password berkala, layaknya sel yang gagal melakukan Quality Control.</p>
          </div>
          <div className={`p-6 rounded-3xl shadow-xl text-center ${darkMode ? 'bg-blue-900/40 border border-blue-800' : 'bg-blue-50 border border-blue-100'}`}>
            <ShieldAlert className="mx-auto text-red-500 mb-2" size={32} />
            <h4 className="font-bold text-2xl text-blue-700 dark:text-blue-300">35%</h4>
            <p className="text-sm text-gray-600 dark:text-gray-400 mt-2">Angka peretasan tinggi membuktikan perlunya "Membran RE" (Firewall & Password Kuat) yang lebih tangguh.</p>
          </div>
        </div>
      </div>
      
      <style dangerouslySetInnerHTML={{__html: `
        @keyframes slideRight { from { width: 0; } }
      `}} />
    </div>
  );
};

/* --- 9. PROFILE VIEW --- */
const ProfileView = ({ darkMode }) => {
  const members = [
    { name: "Anaya Hany Hawari Windani", npm: "2513024032" },
    { name: "Faiqah Kintania Widi", npm: "2513024034" },
    { name: "Annisa Oktaviani Syahri", npm: "2513024058" },
    { name: "Virdha Shafiya Zanita", npm: "2513024070" }
  ];

  const lecturers = [
    "Dr. Pramudiyanti, S.Si., M.Si.",
    "Dr. Dewi Lengkana, M.Sc.",
    "Nadya Meriza, S.Pd., M.Pd."
  ];

  return (
    <div className="animate-fade-in max-w-4xl mx-auto space-y-8">
      <div className="text-center mb-10">
        <div className="inline-block p-4 rounded-full bg-teal-100 text-teal-600 dark:bg-teal-900 dark:text-teal-300 mb-4 shadow-lg">
          <Users size={48} />
        </div>
        <h2 className="text-4xl font-extrabold mb-2">Tim Pengembang</h2>
        <p className="text-xl text-teal-600 dark:text-teal-400 font-semibold">Kelompok 7</p>
      </div>

      <div className="grid md:grid-cols-2 gap-8">
        <div className={`p-8 rounded-3xl shadow-xl border-t-4 border-teal-500 ${darkMode ? 'bg-gray-800' : 'bg-white'}`}>
          <h3 className="text-2xl font-bold mb-6 flex items-center gap-2">
             Anggota Tim
          </h3>
          <ul className="space-y-4">
            {members.map((m, i) => (
              <li key={i} className="flex items-center gap-4 p-3 rounded-xl hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors">
                <div className="w-10 h-10 rounded-full bg-gradient-to-br from-teal-400 to-blue-500 text-white flex items-center justify-center font-bold">
                  {m.name.charAt(0)}
                </div>
                <div>
                  <p className="font-bold">{m.name}</p>
                  <p className="text-sm text-gray-500 dark:text-gray-400">NPM: {m.npm}</p>
                </div>
              </li>
            ))}
          </ul>
        </div>

        <div className={`p-8 rounded-3xl shadow-xl border-t-4 border-blue-500 ${darkMode ? 'bg-gray-800' : 'bg-white'}`}>
          <h3 className="text-2xl font-bold mb-6 flex items-center gap-2">
            Dosen Pengampu
          </h3>
          <ul className="space-y-4">
            {lecturers.map((d, i) => (
              <li key={i} className={`p-4 rounded-xl border ${darkMode ? 'border-gray-700 bg-gray-750' : 'border-gray-100 bg-gray-50'}`}>
                <div className="flex items-center gap-3">
                  <div className="w-2 h-2 rounded-full bg-blue-500"></div>
                  <p className="font-semibold">{d}</p>
                </div>
              </li>
            ))}
          </ul>
          
          <div className="mt-8 pt-6 border-t border-gray-200 dark:border-gray-700 text-center">
            <p className="text-sm text-gray-500 dark:text-gray-400">Program Studi Pendidikan Biologi</p>
            <p className="text-sm text-gray-500 dark:text-gray-400 font-bold mt-1">Universitas Lampung - 2026</p>
          </div>
        </div>

      </div>
    </div>
  );
};

export default App;
