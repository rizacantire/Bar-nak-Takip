import React, { useState, useEffect } from 'react';
import { initializeApp } from 'firebase/app';
import { getAuth, signInAnonymously, onAuthStateChanged, signOut, signInWithCustomToken } from 'firebase/auth';
import { 
  getFirestore, 
  collection, 
  doc, 
  onSnapshot, 
  updateDoc, 
  setDoc,
  getDocs,
  deleteDoc
} from 'firebase/firestore';
import { 
  Settings, 
  CheckCircle, 
  XCircle, 
  Clock, 
  Lock, 
  Unlock,
  RefreshCw,
  LogIn,
  LogOut,
  ChevronRight,
  Plus,
  Trash2
} from 'lucide-react';

// Firebase yapılandırması
const firebaseConfig = JSON.parse(__firebase_config);
const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
const appId = typeof __app_id !== 'undefined' ? __app_id : 'zoo-track-v3';

const INITIAL_ENCLOSURES = [
  { id: 'aslan', name: 'Aslan', status: 'açık' },
  { id: 'kaplan', name: 'Kaplan', status: 'kapalı' },
  { id: 'puma', name: 'Puma', status: 'açık' },
  { id: 'fil', name: 'Fil', status: 'kapalı' },
  { id: 'tropik-merkez', name: 'Tropik Merkez', status: 'açık' },
  { id: 'zurafa', name: 'Zürafa', status: 'açık' }
];

export default function App() {
  const [user, setUser] = useState(null);
  const [isAdmin, setIsAdmin] = useState(false);
  const [view, setView] = useState('login'); 
  const [enclosures, setEnclosures] = useState([]);
  const [loading, setLoading] = useState(true);
  const [message, setMessage] = useState(null);
  
  // Login Form State
  const [email, setEmail] = useState('admin@zoo.com');
  const [password, setPassword] = useState('123456');

  // Yeni Barınak Form State
  const [newEnclosureName, setNewEnclosureName] = useState('');

  // 1. Kimlik Doğrulama Takibi (MANDATORY RULE 3)
  useEffect(() => {
    const initAuth = async () => {
      try {
        if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
          await signInWithCustomToken(auth, __initial_auth_token);
        } else {
          await signInAnonymously(auth);
        }
      } catch (err) {
        console.error("Auth init error:", err);
      }
    };
    initAuth();

    const unsubscribe = onAuthStateChanged(auth, (currentUser) => {
      setUser(currentUser);
      // Admin kontrolü (Simülasyon veya gerçek UID üzerinden)
      if (currentUser && !currentUser.isAnonymous) {
        setIsAdmin(true);
      }
    });
    return () => unsubscribe();
  }, []);

  // 2. Veri Senkronizasyonu (MANDATORY RULE 1 & 2)
  useEffect(() => {
    if (!user) return;

    // Public Path: /artifacts/{appId}/public/data/enclosures
    const enclosuresCol = collection(db, 'artifacts', appId, 'public', 'data', 'enclosures');

    const checkAndSeed = async () => {
      try {
        const snapshot = await getDocs(enclosuresCol);
        if (snapshot.empty) {
          for (const enc of INITIAL_ENCLOSURES) {
            await setDoc(doc(enclosuresCol, enc.id), {
              ...enc,
              lastUpdated: new Date().toISOString(),
              updatedBy: 'Sistem'
            });
          }
        }
      } catch (err) {
        console.error("Seed error:", err);
      }
    };
    checkAndSeed();

    const unsubscribe = onSnapshot(enclosuresCol, 
      (snapshot) => {
        const data = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
        setEnclosures(data.sort((a, b) => a.name.localeCompare(b.name)));
        setLoading(false);
      }, 
      (error) => {
        console.error("Firestore snapshot error:", error);
        setLoading(false);
      }
    );

    return () => unsubscribe();
  }, [user]);

  // Giriş İşlemleri
  const handleLogin = async (e) => {
    e.preventDefault();
    if (email === "admin@zoo.com" && password === "123456") {
      setIsAdmin(true);
      setView('admin');
      showToast("Bakıcı olarak giriş yapıldı.");
    } else {
      showToast("Hatalı bilgiler! (admin@zoo.com / 123456)", "error");
    }
  };

  const handleVisitorAccess = () => {
    setIsAdmin(false);
    setView('visitor');
  };

  const handleLogout = async () => {
    setIsAdmin(false);
    setView('login');
    showToast("Oturum kapatıldı.");
  };

  // Yeni Barınak Ekleme
  const handleAddEnclosure = async (e) => {
    e.preventDefault();
    if (!newEnclosureName.trim() || !isAdmin || !user) {
        showToast("Lütfen barınak ismi girin.", "error");
        return;
    }

    const id = newEnclosureName.toLowerCase()
      .trim()
      .replace(/\s+/g, '-')
      .replace(/[^a-z0-9-şğüıöç]/g, ''); // Türkçe karakter desteği

    if (!id) return;

    // RULE 1: Strict Paths
    const docRef = doc(db, 'artifacts', appId, 'public', 'data', 'enclosures', id);
    
    try {
      await setDoc(docRef, {
        id,
        name: newEnclosureName.trim(),
        status: 'kapalı',
        lastUpdated: new Date().toISOString(),
        updatedBy: user.uid
      });
      setNewEnclosureName('');
      showToast(`${newEnclosureName} başarıyla listeye eklendi.`);
    } catch (error) {
      console.error("Firestore Add Error:", error);
      showToast("Ekleme başarısız! Yetki hatası.", "error");
    }
  };

  // Barınak Silme
  const handleDeleteEnclosure = async (id, name) => {
    if (!isAdmin || !user) return;

    const docRef = doc(db, 'artifacts', appId, 'public', 'data', 'enclosures', id);
    try {
      await deleteDoc(docRef);
      showToast(`${name} silindi.`);
    } catch (error) {
      showToast("Silme hatası!", "error");
    }
  };

  const toggleStatus = async (id, currentStatus) => {
    if (!isAdmin || !user) return;
    
    const newStatus = currentStatus === 'açık' ? 'kapalı' : 'açık';
    const docRef = doc(db, 'artifacts', appId, 'public', 'data', 'enclosures', id);
    
    try {
      await updateDoc(docRef, {
        status: newStatus,
        lastUpdated: new Date().toISOString(),
        updatedBy: user.uid
      });
      showToast(`Durum güncellendi.`);
    } catch (error) {
      showToast("Güncelleme hatası!", "error");
    }
  };

  const showToast = (text, type = "success") => {
    setMessage({ text, type });
    setTimeout(() => setMessage(null), 3000);
  };

  // UI - Giriş Ekranı
  if (view === 'login') {
    return (
      <div className="min-h-screen bg-slate-100 flex items-center justify-center p-4">
        <div className="max-w-md w-full bg-white rounded-3xl shadow-2xl overflow-hidden border border-white">
          <div className="bg-emerald-600 p-10 text-center text-white relative">
            <div className="absolute top-0 right-0 p-4 opacity-10">
                <Settings size={120} />
            </div>
            <div className="inline-block p-4 bg-white/20 backdrop-blur-md rounded-2xl mb-4">
              <Settings className="w-10 h-10" />
            </div>
            <h1 className="text-3xl font-black tracking-tight">ZooTrack</h1>
            <p className="text-emerald-100 mt-2 font-medium tracking-wide">Barınak Kontrol Paneli</p>
          </div>
          
          <div className="p-8">
            <form onSubmit={handleLogin} className="space-y-5">
              <div>
                <label className="block text-xs font-bold text-slate-400 uppercase mb-2 ml-1">Bakıcı E-Posta</label>
                <input 
                  type="email" 
                  value={email}
                  onChange={(e) => setEmail(e.target.value)}
                  className="w-full px-5 py-4 rounded-xl bg-slate-50 border border-slate-200 focus:ring-2 focus:ring-emerald-500 outline-none transition-all font-medium"
                  placeholder="admin@zoo.com"
                />
              </div>
              <div>
                <label className="block text-xs font-bold text-slate-400 uppercase mb-2 ml-1">Şifre</label>
                <input 
                  type="password" 
                  value={password}
                  onChange={(e) => setPassword(e.target.value)}
                  className="w-full px-5 py-4 rounded-xl bg-slate-50 border border-slate-200 focus:ring-2 focus:ring-emerald-500 outline-none transition-all font-medium"
                  placeholder="••••••"
                />
              </div>
              <button 
                type="submit"
                disabled={loading}
                className="w-full bg-emerald-600 text-white py-4 rounded-xl font-bold hover:bg-emerald-700 shadow-lg shadow-emerald-200 transition-all flex items-center justify-center gap-2 text-lg"
              >
                {loading ? <RefreshCw className="w-6 h-6 animate-spin" /> : <LogIn className="w-6 h-6" />}
                Giriş Yap
              </button>
            </form>

            <div className="relative my-10 text-center">
              <div className="absolute inset-0 flex items-center"><div className="w-full border-t border-slate-100"></div></div>
              <span className="relative bg-white px-4 text-xs font-bold text-slate-300 uppercase tracking-widest">VEYA</span>
            </div>

            <button 
              onClick={handleVisitorAccess}
              className="w-full bg-white text-slate-600 py-4 rounded-xl font-bold hover:bg-slate-50 border-2 border-slate-100 transition-all flex items-center justify-center gap-2 group"
            >
              Ziyaretçi Olarak Devam Et
              <ChevronRight className="w-5 h-5 group-hover:translate-x-1 transition-transform text-emerald-500" />
            </button>
          </div>
        </div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-slate-50 text-slate-900 font-sans pb-20">
      <header className="bg-white/90 backdrop-blur-md shadow-sm border-b sticky top-0 z-30">
        <div className="max-w-6xl mx-auto px-4 h-20 flex items-center justify-between">
          <div className="flex items-center gap-3">
            <div className="bg-emerald-600 p-2.5 rounded-xl text-white shadow-lg shadow-emerald-100">
              <Settings className="w-6 h-6" />
            </div>
            <div>
                <h1 className="text-xl font-black tracking-tight leading-none text-slate-800">ZooTrack</h1>
                <span className="text-[10px] font-bold text-emerald-600 uppercase tracking-widest">Merkezi Sistem</span>
            </div>
          </div>
          
          <div className="flex items-center gap-3">
            <div className={`hidden md:flex items-center gap-2 px-4 py-2 rounded-full border text-xs font-bold ${
              isAdmin ? 'bg-amber-50 border-amber-200 text-amber-700' : 'bg-blue-50 border-blue-200 text-blue-700'
            }`}>
              {isAdmin ? 'BAKICI YETKİSİ' : 'ZİYARETÇİ MODU'}
            </div>
            <button 
              onClick={handleLogout}
              className="flex items-center gap-2 px-4 py-2 bg-slate-100 hover:bg-red-50 hover:text-red-600 rounded-xl transition-all text-sm font-bold text-slate-500"
            >
              <LogOut className="w-4 h-4" />
              <span>Çıkış</span>
            </button>
          </div>
        </div>
      </header>

      <main className="max-w-6xl mx-auto px-4 mt-8">
        {/* Admin Form: Add New Enclosure */}
        {isAdmin && (
          <div className="bg-white rounded-3xl p-6 mb-8 border border-slate-200 shadow-sm animate-in fade-in slide-in-from-top-4 duration-500">
            <h2 className="text-lg font-bold mb-4 flex items-center gap-2">
                <Plus className="w-5 h-5 text-emerald-600" />
                Yeni Barınak Ekle
            </h2>
            <form onSubmit={handleAddEnclosure} className="flex flex-col sm:flex-row gap-3">
              <input 
                type="text" 
                value={newEnclosureName}
                onChange={(e) => setNewEnclosureName(e.target.value)}
                placeholder="Barınak adını yazın (örn: Kaplan)"
                className="flex-1 px-5 py-3 rounded-xl bg-slate-50 border border-slate-200 focus:ring-2 focus:ring-emerald-500 outline-none transition-all font-medium"
              />
              <button 
                type="submit"
                className="bg-emerald-600 text-white px-8 py-3 rounded-xl font-bold hover:bg-emerald-700 transition-all flex items-center justify-center gap-2 shadow-lg shadow-emerald-100"
              >
                Ekle
              </button>
            </form>
          </div>
        )}

        <div className={`rounded-3xl p-8 mb-10 text-white shadow-xl relative overflow-hidden ${
          isAdmin ? 'bg-gradient-to-br from-amber-500 to-orange-600' : 'bg-gradient-to-br from-emerald-600 to-teal-700'
        }`}>
          <div className="relative z-10 max-w-2xl">
            <h2 className="text-3xl font-black mb-3 leading-tight">
              {isAdmin ? 'Hoş Geldiniz, Bakıcı' : 'Ziyaretçi Paneli'}
            </h2>
            <p className="text-white/80 font-medium text-lg">
              {isAdmin 
                ? 'Barınakları yönetin, durumları güncelleyin veya yeni barınak ekleyin.' 
                : 'Hangi hayvanların ziyaret için uygun olduğunu aşağıdan takip edebilirsiniz.'}
            </p>
          </div>
        </div>

        {/* Enclosure Grid */}
        <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
          {enclosures.map((enc) => (
            <div 
              key={enc.id} 
              className="bg-white rounded-[2.5rem] shadow-sm border border-slate-200 overflow-hidden hover:shadow-xl transition-all duration-300 group"
            >
              <div className="p-8">
                <div className="flex justify-between items-start mb-4">
                  <div className="flex-1">
                    <div className="flex items-center gap-2">
                        <h3 className="text-xl font-black text-slate-800 leading-none">{enc.name}</h3>
                        {/* Ziyaretçi için durum yazısı */}
                        <span className={`text-[10px] font-black px-2 py-0.5 rounded uppercase ${
                            enc.status === 'açık' ? 'text-green-600 bg-green-50' : 'text-red-600 bg-red-50'
                        }`}>
                            {enc.status}
                        </span>
                    </div>
                    <div className="flex items-center gap-1.5 mt-2 text-[10px] font-bold text-slate-400 uppercase">
                      <Clock className="w-3 h-3" />
                      <span>{new Date(enc.lastUpdated).toLocaleTimeString('tr-TR')}</span>
                    </div>
                  </div>
                  {isAdmin && (
                    <button 
                        onClick={() => handleDeleteEnclosure(enc.id, enc.name)}
                        className="p-2 text-slate-300 hover:text-red-500 transition-colors"
                    >
                        <Trash2 className="w-4 h-4" />
                    </button>
                  )}
                </div>

                <div className="flex items-center gap-2 mb-6">
                    <div className={`w-full py-2 rounded-full text-center text-[10px] font-black uppercase tracking-widest flex items-center justify-center gap-2 ${
                        enc.status === 'açık' ? 'bg-green-100 text-green-700' : 'bg-red-100 text-red-700'
                    }`}>
                        <div className={`w-2 h-2 rounded-full ${enc.status === 'açık' ? 'bg-green-500' : 'bg-red-500 animate-pulse'}`} />
                        BARINAK {enc.status}
                    </div>
                </div>

                {isAdmin ? (
                  <button
                    onClick={() => toggleStatus(enc.id, enc.status)}
                    className={`w-full py-4 rounded-2xl font-black text-xs transition-all flex items-center justify-center gap-2 border-2 uppercase tracking-widest ${
                      enc.status === 'açık'
                        ? 'border-red-100 bg-red-50 text-red-600 hover:bg-red-100'
                        : 'border-green-100 bg-green-50 text-green-600 hover:bg-green-100'
                    }`}
                  >
                    {enc.status === 'açık' ? 'Kapat' : 'Aç'}
                  </button>
                ) : (
                  <div className={`py-4 px-4 rounded-2xl text-center text-xs font-black uppercase tracking-widest border ${
                    enc.status === 'açık' ? 'bg-emerald-50 text-emerald-600 border-emerald-100' : 'bg-slate-50 text-slate-400 border-slate-200'
                  }`}>
                    {enc.status === 'açık' ? 'Ziyaret Edilebilir' : 'Şu an Kapalı'}
                  </div>
                )}
              </div>
              <div className={`h-2 w-full ${enc.status === 'açık' ? 'bg-emerald-500' : 'bg-red-500'}`} />
            </div>
          ))}
        </div>
      </main>

      {/* Toast Notification */}
      {message && (
        <div className="fixed bottom-10 left-1/2 -translate-x-1/2 z-50 animate-bounce">
          <div className={`px-8 py-4 rounded-2xl shadow-2xl text-white font-black flex items-center gap-4 ${
            message.type === 'success' ? 'bg-slate-900' : 'bg-red-600'
          }`}>
            {message.type === 'success' ? <CheckCircle className="w-5 h-5 text-emerald-400" /> : <XCircle className="w-5 h-5" />}
            {message.text}
          </div>
        </div>
      )}
    </div>
  );
}
