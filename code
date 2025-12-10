import React, { useState, useEffect, useRef } from 'react';
import { Shield, Wifi, WifiOff, Zap, Database, Lock, Server, Smartphone, CheckCircle, Clock, User } from 'lucide-react';

// --- Mock Data Generators ---
const generateIC = () => {
  const year = Math.floor(Math.random() * (99 - 50) + 50);
  const month = String(Math.floor(Math.random() * 12) + 1).padStart(2, '0');
  const day = String(Math.floor(Math.random() * 28) + 1).padStart(2, '0');
  const tail = String(Math.floor(Math.random() * 8999) + 1000);
  return `${year}${month}${day}-10-${tail}`;
};

// Simple pseudo-hashing for visual demo purposes
const mockHash = (input) => {
  // Simulating SHA-256 output structure
  const chars = '0123456789abcdef';
  let result = 'x9#m2'; 
  for (let i = 0; i < 20; i++) {
    result += chars[Math.floor(Math.random() * chars.length)];
  }
  return result + '...';
};

const MyMeritDemo = () => {
  // System State
  const [isOnline, setIsOnline] = useState(true);
  const [isRapidMode, setIsRapidMode] = useState(false);
  
  // Data State
  const [localDb, setLocalDb] = useState([]);
  const [cloudDb, setCloudDb] = useState([]);
  const [currentScan, setCurrentScan] = useState(null);
  
  // Statistics
  const [scanCount, setScanCount] = useState(0);
  const [scanTime, setScanTime] = useState(0);

  // References for animation loops
  const queueInterval = useRef(null);
  const syncInterval = useRef(null);

  // --- CORE LOGIC: THE SCAN ---
  const handleScan = (isRapid = false) => {
    const rawId = generateIC();
    const timestamp = new Date().toLocaleTimeString();
    
    // 1. Immediate Hash (Privacy Vault)
    const eventKey = "EVT-2023-KUL";
    const hashedId = mockHash(rawId + eventKey);
    
    const newRecord = {
      id: Date.now(),
      rawId: "HIDDEN", // We never store raw
      displayRaw: rawId, // Just for the visual "flash" animation
      hash: hashedId,
      timestamp: timestamp,
      status: 'encrypted_stored'
    };

    // Trigger UI Flash
    setCurrentScan(newRecord);
    
    // Add to Local DB (Zero-Internet Core)
    setLocalDb(prev => [newRecord, ...prev]);
    setScanCount(prev => prev + 1);

    // If rapid mode, minimize UI flash duration
    if (!isRapid) {
      setTimeout(() => setCurrentScan(null), 1500);
    }
  };

  // --- RAPID FIRE MODE ---
  const toggleRapidFire = () => {
    if (isRapidMode) {
      setIsRapidMode(false);
      clearInterval(queueInterval.current);
    } else {
      setIsRapidMode(true);
      // Simulate 0.5s scan time
      queueInterval.current = setInterval(() => {
        handleScan(true);
      }, 500);
    }
  };

  // --- BACKGROUND SYNC (Zero-Internet Core) ---
  useEffect(() => {
    if (isOnline && localDb.length > 0) {
      // Simulate "Silent Sync"
      syncInterval.current = setInterval(() => {
        setLocalDb(prev => {
          if (prev.length === 0) return prev;
          const [toSync, ...rest] = prev;
          
          // Move to cloud
          setCloudDb(cloudPrev => [{...toSync, status: 'synced'}, ...cloudPrev]);
          return rest;
        });
      }, 200); // Fast sync when connection restored
    } else {
      clearInterval(syncInterval.current);
    }

    return () => clearInterval(syncInterval.current);
  }, [isOnline, localDb]);

  // Cleanup on unmount
  useEffect(() => {
    return () => {
      clearInterval(queueInterval.current);
      clearInterval(syncInterval.current);
    };
  }, []);

  return (
    <div className="min-h-screen bg-slate-900 text-slate-100 p-4 md:p-8 font-sans">
      
      {/* HEADER / PITCH CONTEXT */}
      <div className="max-w-6xl mx-auto mb-8 flex flex-col md:flex-row justify-between items-center border-b border-slate-700 pb-6">
        <div>
          <h1 className="text-3xl font-bold text-white flex items-center gap-2">
            <Shield className="text-blue-500" /> MyMerit Touch
            <span className="text-xs bg-blue-600 text-white px-2 py-1 rounded ml-2">DEMO ENV</span>
          </h1>
          <p className="text-slate-400 mt-2 text-sm">
            Core: Kotlin Native | Protocol: ISO/IEC 7816-4 | Security: AES-256
          </p>
        </div>
        
        <div className="flex items-center gap-4 mt-4 md:mt-0 bg-slate-800 p-3 rounded-xl border border-slate-700">
          <div className="text-right mr-2">
            <p className="text-xs text-slate-400">Network Status</p>
            <p className={`font-bold ${isOnline ? 'text-green-400' : 'text-red-400'}`}>
              {isOnline ? 'ONLINE (4G/LTE)' : 'OFFLINE (Air-gapped)'}
            </p>
          </div>
          <button 
            onClick={() => setIsOnline(!isOnline)}
            className={`p-3 rounded-full transition-all ${isOnline ? 'bg-green-500/20 text-green-400' : 'bg-red-500/20 text-red-400'}`}
          >
            {isOnline ? <Wifi size={24} /> : <WifiOff size={24} />}
          </button>
        </div>
      </div>

      <div className="max-w-6xl mx-auto grid grid-cols-1 lg:grid-cols-3 gap-8">
        
        {/* LEFT COLUMN: CONTROLS & SCANNER */}
        <div className="lg:col-span-1 space-y-6">
          
          {/* SCANNER SIMULATION CARD */}
          <div className="bg-slate-800 rounded-2xl p-6 border border-slate-700 shadow-xl relative overflow-hidden">
            <div className="absolute top-0 left-0 w-full h-1 bg-blue-500"></div>
            <h2 className="text-xl font-bold mb-4 flex items-center gap-2">
              <Smartphone size={20} className="text-blue-400" />
              Field Device
            </h2>

            {/* Visualizer Area */}
            <div className="bg-slate-900 rounded-xl p-6 mb-6 flex flex-col items-center justify-center h-48 border border-dashed border-slate-700 relative">
              {currentScan ? (
                <div className="text-center animate-in fade-in zoom-in duration-300">
                  <div className="flex justify-center mb-2">
                    <Lock className="text-green-400 h-10 w-10 animate-bounce" />
                  </div>
                  <p className="text-xs text-red-400 line-through opacity-50 mb-1">{currentScan.displayRaw}</p>
                  <p className="text-lg font-mono text-green-400 font-bold">{currentScan.hash}</p>
                  <p className="text-xs text-slate-500 mt-2">SHA-256 Hashed + Salted</p>
                </div>
              ) : (
                <div className="text-center opacity-30">
                  <User size={48} className="mx-auto mb-2" />
                  <p>Ready to Scan</p>
                </div>
              )}
            </div>

            {/* Actions */}
            <div className="space-y-3">
              <button 
                onClick={() => handleScan(false)}
                disabled={isRapidMode}
                className="w-full bg-blue-600 hover:bg-blue-500 active:scale-95 transition-all text-white font-bold py-3 px-4 rounded-xl flex items-center justify-center gap-2 disabled:opacity-50 disabled:cursor-not-allowed"
              >
                Scan Single Identity
              </button>
              
              <button 
                onClick={toggleRapidFire}
                className={`w-full border-2 transition-all font-bold py-3 px-4 rounded-xl flex items-center justify-center gap-2 
                  ${isRapidMode 
                    ? 'border-red-500 text-red-400 bg-red-500/10 animate-pulse' 
                    : 'border-slate-600 text-slate-300 hover:border-blue-400 hover:text-white'
                  }`}
              >
                <Zap size={18} />
                {isRapidMode ? 'STOP Rapid-Fire' : 'Simulate 500 Villagers Queue'}
              </button>
            </div>

            <div className="mt-6 pt-6 border-t border-slate-700">
              <div className="flex justify-between items-center text-sm">
                <span className="text-slate-400">Scan Speed:</span>
                <span className="text-green-400 font-mono">~0.5s / unit</span>
              </div>
              <div className="flex justify-between items-center text-sm mt-2">
                <span className="text-slate-400">Processor:</span>
                <span className="text-slate-200">APDU Optimized</span>
              </div>
            </div>
          </div>

        </div>

        {/* RIGHT COLUMN: DATA FLOW VISUALIZATION */}
        <div className="lg:col-span-2 grid grid-cols-1 md:grid-cols-2 gap-6">
          
          {/* LOCAL STORAGE */}
          <div className="bg-slate-800 rounded-2xl p-0 border border-slate-700 shadow-lg flex flex-col h-[500px]">
            <div className="p-4 border-b border-slate-700 bg-slate-800/50 backdrop-blur rounded-t-2xl flex justify-between items-center sticky top-0 z-10">
              <h3 className="font-bold flex items-center gap-2">
                <Database size={18} className="text-orange-400" />
                Local Encrypted DB
              </h3>
              <span className="bg-orange-500/20 text-orange-400 text-xs px-2 py-1 rounded-full">
                {localDb.length} Pending
              </span>
            </div>
            
            <div className="flex-1 overflow-y-auto p-4 space-y-2 relative">
              {localDb.length === 0 && (
                <div className="flex flex-col items-center justify-center h-full text-slate-600">
                  <Database size={40} className="mb-2 opacity-20" />
                  <p className="text-sm">Storage Empty</p>
                </div>
              )}
              {localDb.map((record) => (
                <div key={record.id} className="bg-slate-900/50 p-3 rounded-lg border border-slate-700/50 flex items-center justify-between animate-in slide-in-from-left duration-300">
                  <div>
                    <div className="flex items-center gap-2">
                      <Lock size={12} className="text-green-500" />
                      <span className="font-mono text-xs text-green-400">{record.hash}</span>
                    </div>
                    <p className="text-[10px] text-slate-500 mt-1">AES-256 • {record.timestamp}</p>
                  </div>
                  <div className="h-2 w-2 rounded-full bg-orange-500 animate-pulse"></div>
                </div>
              ))}
            </div>
            {/* Overlay for offline indication */}
            {!isOnline && (
              <div className="p-2 bg-orange-500/10 border-t border-orange-500/20 text-center text-xs text-orange-400 font-bold">
                STORED OFFLINE (WorkManager Queue)
              </div>
            )}
          </div>

          {/* CLOUD STORAGE */}
          <div className="bg-slate-800 rounded-2xl p-0 border border-slate-700 shadow-lg flex flex-col h-[500px]">
            <div className="p-4 border-b border-slate-700 bg-slate-800/50 backdrop-blur rounded-t-2xl flex justify-between items-center sticky top-0 z-10">
              <h3 className="font-bold flex items-center gap-2">
                <Server size={18} className="text-blue-400" />
                HQ Cloud Server
              </h3>
              <span className="bg-blue-500/20 text-blue-400 text-xs px-2 py-1 rounded-full">
                {cloudDb.length} Synced
              </span>
            </div>

            <div className="flex-1 overflow-y-auto p-4 space-y-2">
              {cloudDb.length === 0 && (
                <div className="flex flex-col items-center justify-center h-full text-slate-600">
                  <Server size={40} className="mb-2 opacity-20" />
                  <p className="text-sm">Waiting for Sync...</p>
                </div>
              )}
              {cloudDb.map((record) => (
                <div key={record.id} className="bg-blue-900/10 p-3 rounded-lg border border-blue-500/20 flex items-center justify-between animate-in slide-in-from-bottom duration-500">
                  <div>
                    <span className="font-mono text-xs text-blue-300">{record.hash}</span>
                    <p className="text-[10px] text-slate-500 mt-1">Synced via REST API</p>
                  </div>
                  <CheckCircle size={14} className="text-blue-500" />
                </div>
              ))}
            </div>
          </div>

        </div>
      </div>
      
      {/* Footer Stats */}
      <div className="max-w-6xl mx-auto mt-8 grid grid-cols-3 gap-4 text-center">
         <div className="bg-slate-800 p-4 rounded-xl border border-slate-700">
            <p className="text-slate-400 text-xs uppercase tracking-wider">Total Scanned</p>
            <p className="text-3xl font-bold text-white">{scanCount}</p>
         </div>
         <div className="bg-slate-800 p-4 rounded-xl border border-slate-700">
            <p className="text-slate-400 text-xs uppercase tracking-wider">Privacy Leaks</p>
            <p className="text-3xl font-bold text-green-500">0</p>
         </div>
         <div className="bg-slate-800 p-4 rounded-xl border border-slate-700">
            <p className="text-slate-400 text-xs uppercase tracking-wider">Avg Latency</p>
            <p className="text-3xl font-bold text-blue-400">~480ms</p>
         </div>
      </div>
    </div>
  );
};

export default MyMeritDemo;
