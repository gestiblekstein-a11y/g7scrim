<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MLBB Scrim & Tourney Poster Generator</title>
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Google Fonts for E-Sports Vibe -->
    <link href="https://fonts.googleapis.com/css2?family=Rajdhani:wght@400;600;700&family=Teko:wght@400;600;700&display=swap" rel="stylesheet">
    
    <!-- FontAwesome for Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

    <!-- html2canvas for downloading the poster -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        rajdhani: ['Rajdhani', 'sans-serif'],
                        teko: ['Teko', 'sans-serif'],
                    },
                    colors: {
                        esports: {
                            dark: '#0f172a',
                            panel: '#1e293b',
                            blue: '#3b82f6',
                            red: '#ef4444',
                            gold: '#fbbf24'
                        }
                    }
                }
            }
        }
    </script>
    
    <style>
        body {
            background-color: #020617;
            color: #f8fafc;
        }
        
        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #1e293b; 
        }
        ::-webkit-scrollbar-thumb {
            background: #475569; 
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #64748b; 
        }

        /* Poster Background styling to ensure good export */
        #poster-preview {
            background: linear-gradient(135deg, #0f172a 0%, #000000 100%);
            position: relative;
            overflow: hidden;
            border: 1px solid #334155;
        }
        
        #poster-preview::before {
            content: '';
            position: absolute;
            top: 0; left: 0; right: 0; bottom: 0;
            background-image: 
                linear-gradient(rgba(255, 255, 255, 0.03) 1px, transparent 1px),
                linear-gradient(90deg, rgba(255, 255, 255, 0.03) 1px, transparent 1px);
            background-size: 20px 20px;
            z-index: 1;
        }

        .poster-content {
            position: relative;
            z-index: 10;
        }

        /* Input styling */
        .input-game {
            background: #0f172a;
            border: 1px solid #334155;
            color: #fff;
            transition: all 0.2s;
        }
        .input-game:focus {
            border-color: #3b82f6;
            outline: none;
            box-shadow: 0 0 10px rgba(59, 130, 246, 0.3);
        }
        
        .role-icon-box {
            width: 32px;
            height: 32px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 4px;
            background: #1e293b;
            color: #fbbf24;
            font-size: 14px;
        }
    </style>
</head>
<body class="min-h-screen font-rajdhani text-slate-200">

    <header class="bg-esports-panel border-b border-slate-700 py-4 shadow-lg mb-6">
        <div class="container mx-auto px-4 flex justify-between items-center">
            <h1 class="text-3xl font-teko text-esports-gold font-bold tracking-wider">
                <i class="fa-solid fa-gamepad mr-2 text-white"></i> MLBB MATCH GENERATOR
            </h1>
            <p class="text-sm text-slate-400 hidden sm:block">Create e-sports ready announcements</p>
        </div>
    </header>

    <main class="container mx-auto px-4 pb-12">
        <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
            
            <!-- LEFT PANEL: CONTROLS -->
            <div class="lg:col-span-5 space-y-6">
                
                <!-- Match Info Settings -->
                <div class="bg-esports-panel p-5 rounded-xl shadow-xl border border-slate-700">
                    <h2 class="text-xl font-bold mb-4 text-white border-b border-slate-600 pb-2">
                        <i class="fa-solid fa-calendar-day mr-2 text-esports-blue"></i> Detail Pertandingan
                    </h2>
                    
                    <div class="grid grid-cols-2 gap-4 mb-4">
                        <div>
                            <label class="block text-sm text-slate-400 mb-1">Tipe Pertandingan</label>
                            <input type="text" id="inputType" class="w-full input-game rounded p-2 text-sm font-bold" value="FRIENDLY SCRIM" oninput="updatePoster()" placeholder="Contoh: FRIENDLY SCRIM">
                        </div>
                        <div>
                            <label class="block text-sm text-slate-400 mb-1">Format (BO)</label>
                            <select id="inputFormat" class="w-full input-game rounded p-2 text-sm font-bold" onchange="updatePoster()">
                                <option value="BO1">BO 1</option>
                                <option value="BO3" selected>BO 3</option>
                                <option value="BO5">BO 5</option>
                                <option value="4 GAME">4 GAME</option>
                            </select>
                        </div>
                    </div>
                    
                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <label class="block text-sm text-slate-400 mb-1">Tanggal</label>
                            <input type="date" id="inputDate" class="w-full input-game rounded p-2 text-sm" value="2026-10-05" oninput="updatePoster()" onchange="updatePoster()">
                        </div>
                        <div>
                            <label class="block text-sm text-slate-400 mb-1">Waktu (WIB)</label>
                            <input type="time" id="inputTime" class="w-full input-game rounded p-2 text-sm" value="19:30" oninput="updatePoster()" onchange="updatePoster()">
                        </div>
                    </div>
                </div>

                <!-- Team A Roster -->
                <div class="bg-esports-panel p-5 rounded-xl shadow-xl border border-slate-700">
                    <h2 class="text-xl font-bold mb-4 text-esports-blue border-b border-slate-600 pb-2">
                        <i class="fa-solid fa-shield mr-2"></i> Tim A (Kiri)
                    </h2>
                    <div class="mb-4">
                        <label class="block text-sm text-slate-400 mb-1">Nama Tim</label>
                        <input type="text" id="inputTeamA" class="w-full input-game rounded p-2 text-lg font-bold text-center" value="" placeholder="NAMA TIM A" oninput="updatePoster()">
                    </div>
                    
                    <div class="space-y-3" id="rosterA">
                        <!-- Roles will be generated via JS -->
                    </div>
                </div>

                <!-- Team B Roster -->
                <div class="bg-esports-panel p-5 rounded-xl shadow-xl border border-slate-700">
                    <h2 class="text-xl font-bold mb-4 text-esports-red border-b border-slate-600 pb-2">
                        <i class="fa-solid fa-shield mr-2"></i> Tim B (Kanan)
                    </h2>
                    <div class="mb-4">
                        <label class="block text-sm text-slate-400 mb-1">Nama Tim</label>
                        <input type="text" id="inputTeamB" class="w-full input-game rounded p-2 text-lg font-bold text-center" value="" placeholder="NAMA TIM B" oninput="updatePoster()">
                    </div>
                    
                    <div class="space-y-3" id="rosterB">
                        <!-- Roles will be generated via JS -->
                    </div>
                </div>

            </div>

            <!-- RIGHT PANEL: PREVIEW -->
            <div class="lg:col-span-7 flex flex-col items-center">
                
                <div class="w-full flex justify-between items-center mb-4">
                    <h2 class="text-2xl font-bold font-teko tracking-wide">LIVE PREVIEW</h2>
                    <button onclick="downloadPoster()" class="bg-esports-blue hover:bg-blue-600 text-white px-4 py-2 rounded font-bold shadow-[0_0_15px_rgba(59,130,246,0.5)] transition-all flex items-center">
                        <i class="fa-solid fa-download mr-2"></i> Download Poster
                    </button>
                </div>

                <!-- THE POSTER CONTAINER (This is what gets captured) -->
                <div id="poster-preview" class="w-full aspect-[4/5] max-w-[600px] rounded-xl shadow-2xl flex flex-col justify-between p-8 text-center mx-auto">
                    
                    <div class="poster-content w-full h-full flex flex-col">
                        
                        <!-- Header -->
                        <div class="mb-8">
                            <h3 id="outType" class="text-esports-gold font-teko text-5xl font-bold tracking-widest drop-shadow-[0_0_10px_rgba(251,191,36,0.8)] uppercase">
                                FRIENDLY SCRIM
                            </h3>
                            <div class="inline-block bg-slate-800/80 px-4 py-1 rounded-full border border-slate-600 mt-2">
                                <span id="outFormat" class="text-white font-bold tracking-widest text-sm uppercase">BEST OF 3</span>
                            </div>
                        </div>

                        <!-- Teams VS Area -->
                        <div class="flex items-center justify-between w-full mb-8 relative">
                            <!-- Team A Name -->
                            <div class="w-2/5">
                                <h2 id="outTeamA" class="font-teko text-4xl sm:text-5xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-blue-400 to-blue-200 drop-shadow-[0_0_15px_rgba(59,130,246,0.6)] uppercase break-words leading-none">
                                    EVOS GLORY
                                </h2>
                            </div>
                            
                            <!-- VS Marker -->
                            <div class="w-1/5 flex justify-center z-20">
                                <div class="bg-gradient-to-br from-gray-800 to-black w-16 h-16 sm:w-20 sm:h-20 rounded-full flex items-center justify-center border-2 border-slate-500 shadow-[0_0_30px_rgba(255,255,255,0.2)]">
                                    <span class="font-teko text-3xl sm:text-4xl font-bold italic text-white drop-shadow-[0_0_5px_rgba(255,255,255,0.8)]">VS</span>
                                </div>
                            </div>

                            <!-- Team B Name -->
                            <div class="w-2/5">
                                <h2 id="outTeamB" class="font-teko text-4xl sm:text-5xl font-bold text-transparent bg-clip-text bg-gradient-to-l from-red-500 to-red-300 drop-shadow-[0_0_15px_rgba(239,68,68,0.6)] uppercase break-words leading-none">
                                    RRQ HOSHI
                                </h2>
                            </div>
                        </div>

                        <!-- Schedule -->
                        <div class="bg-black/60 border border-slate-700/50 backdrop-blur-sm rounded-xl py-3 px-6 mb-8 inline-flex flex-col sm:flex-row gap-4 sm:gap-8 mx-auto items-center">
                            <div class="flex items-center text-slate-200 text-lg sm:text-xl font-bold tracking-wide">
                                <i class="fa-regular fa-calendar text-esports-gold mr-3 text-2xl"></i>
                                <span id="outDate">Sabtu, 24 Agustus 2026</span>
                            </div>
                            <div class="hidden sm:block w-px h-8 bg-slate-600"></div>
                            <div class="flex items-center text-slate-200 text-lg sm:text-xl font-bold tracking-wide">
                                <i class="fa-regular fa-clock text-esports-gold mr-3 text-2xl"></i>
                                <span id="outTime">19:30 WIB</span>
                            </div>
                        </div>

                        <!-- Rosters -->
                        <div class="flex-grow flex justify-between w-full mt-4">
                            
                            <!-- Team A Roster List -->
                            <div class="w-[45%] text-left" id="outRosterA">
                                <!-- Generated by JS -->
                            </div>
                            
                            <!-- Team B Roster List -->
                            <div class="w-[45%] text-right" id="outRosterB">
                                <!-- Generated by JS -->
                            </div>
                            
                        </div>

                        <!-- Footer styling for poster -->
                        <div class="mt-8 pt-4 border-t border-slate-800 text-slate-500 text-sm font-semibold tracking-widest">
                            BY G7 OFFICIAL
                        </div>

                    </div>
                </div>
                
                <p class="text-sm text-slate-500 mt-4"><i class="fa-solid fa-circle-info mr-1"></i> Disarankan menggunakan rasio layar Desktop untuk hasil download terbaik.</p>
            </div>
        </div>
    </main>

    <script>
        // Data Struktur untuk Role
        const roles = [
            { id: 'exp', name: 'EXP LANE', textColor: 'text-amber-500' },
            { id: 'jungle', name: 'JUNGLER', textColor: 'text-purple-400' },
            { id: 'mid', name: 'MID LANE', textColor: 'text-blue-400' },
            { id: 'gold', name: 'GOLD LANE', textColor: 'text-yellow-400' },
            { id: 'roam', name: 'ROAM', textColor: 'text-emerald-400' }
        ];

        // Default Players Team A
        const defaultTeamA = ['', '', '', '', ''];
        // Default Players Team B
        const defaultTeamB = ['', '', '', '', ''];

        // Render form inputs for rosters
        function renderRosterInputs() {
            const rosterA = document.getElementById('rosterA');
            const rosterB = document.getElementById('rosterB');
            
            let htmlA = '';
            let htmlB = '';

            roles.forEach((role, index) => {
                // Team A Inputs
                htmlA += `
                    <div class="flex items-center gap-2">
                        <div class="w-20 text-[10px] sm:text-xs font-bold text-center ${role.textColor} bg-slate-800 rounded py-2 px-1 border border-slate-700 shrink-0" title="${role.name}">
                            ${role.name}
                        </div>
                        <input type="text" id="p_a_${role.id}" class="w-full input-game rounded p-2 text-sm uppercase" placeholder="Nickname" value="${defaultTeamA[index]}" oninput="updatePoster()">
                    </div>
                `;

                // Team B Inputs
                htmlB += `
                    <div class="flex items-center gap-2">
                        <input type="text" id="p_b_${role.id}" class="w-full input-game rounded p-2 text-sm text-right uppercase" placeholder="Nickname" value="${defaultTeamB[index]}" oninput="updatePoster()">
                        <div class="w-20 text-[10px] sm:text-xs font-bold text-center ${role.textColor} bg-slate-800 rounded py-2 px-1 border border-slate-700 shrink-0" title="${role.name}">
                            ${role.name}
                        </div>
                    </div>
                `;
            });

            rosterA.innerHTML = htmlA;
            rosterB.innerHTML = htmlB;
        }

        // Update Poster Canvas from inputs
        function updatePoster() {
            // General Info
            document.getElementById('outType').innerText = document.getElementById('inputType').value || 'MABAR';
            document.getElementById('outFormat').innerText = document.getElementById('inputFormat').value || '-';
            
            // Format Tanggal ke Bahasa Indonesia
            const rawDate = document.getElementById('inputDate').value;
            let formattedDate = '-';
            if (rawDate) {
                // Mengkonversi format YYYY-MM-DD menjadi "Hari, Tanggal Bulan Tahun"
                const dateObj = new Date(rawDate);
                const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
                formattedDate = dateObj.toLocaleDateString('id-ID', options);
            }
            document.getElementById('outDate').innerText = formattedDate;

            // Format Waktu
            const rawTime = document.getElementById('inputTime').value;
            document.getElementById('outTime').innerText = rawTime ? rawTime + ' WIB' : '-';
            
            // Team Names
            document.getElementById('outTeamA').innerText = document.getElementById('inputTeamA').value || 'TEAM A';
            document.getElementById('outTeamB').innerText = document.getElementById('inputTeamB').value || 'TEAM B';

            // Update Rosters Output
            let outHtmlA = '';
            let outHtmlB = '';

            roles.forEach(role => {
                let nameA = document.getElementById(`p_a_${role.id}`).value || '-';
                let nameB = document.getElementById(`p_b_${role.id}`).value || '-';

                // Team A Poster Row
                outHtmlA += `
                    <div class="flex items-center gap-2 sm:gap-3 mb-3 bg-gradient-to-r from-blue-900/40 to-transparent p-2 rounded border-l-2 border-blue-500">
                        <div class="w-16 sm:w-20 text-[10px] sm:text-xs font-bold text-center ${role.textColor} tracking-wider shrink-0 bg-slate-900/60 rounded py-1 border border-slate-700/50">
                            ${role.name}
                        </div>
                        <div class="font-bold text-base sm:text-lg text-white truncate uppercase tracking-wider">${nameA}</div>
                    </div>
                `;

                // Team B Poster Row (Reversed)
                outHtmlB += `
                    <div class="flex items-center gap-2 sm:gap-3 mb-3 justify-end bg-gradient-to-l from-red-900/40 to-transparent p-2 rounded border-r-2 border-red-500">
                        <div class="font-bold text-base sm:text-lg text-white truncate uppercase tracking-wider text-right">${nameB}</div>
                        <div class="w-16 sm:w-20 text-[10px] sm:text-xs font-bold text-center ${role.textColor} tracking-wider shrink-0 bg-slate-900/60 rounded py-1 border border-slate-700/50">
                            ${role.name}
                        </div>
                    </div>
                `;
            });

            document.getElementById('outRosterA').innerHTML = outHtmlA;
            document.getElementById('outRosterB').innerHTML = outHtmlB;
        }

        // Download logic using html2canvas
        function downloadPoster() {
            const previewDiv = document.getElementById('poster-preview');
            const btn = event.currentTarget;
            const originalText = btn.innerHTML;
            
            // UI feedback
            btn.innerHTML = '<i class="fa-solid fa-spinner fa-spin mr-2"></i> Generating...';
            btn.disabled = true;

            // Use html2canvas
            html2canvas(previewDiv, {
                scale: 2, // High resolution
                backgroundColor: "#020617", // match background
                useCORS: true,
                logging: false
            }).then(canvas => {
                // Create download link
                const link = document.createElement('a');
                
                // Construct filename based on teams
                const teamA = document.getElementById('inputTeamA').value.replace(/[^a-z0-9]/gi, '_').toLowerCase() || 'teama';
                const teamB = document.getElementById('inputTeamB').value.replace(/[^a-z0-9]/gi, '_').toLowerCase() || 'teamb';
                
                link.download = `Match_${teamA}_vs_${teamB}.png`;
                link.href = canvas.toDataURL('image/png');
                link.click();

                // Restore UI
                btn.innerHTML = originalText;
                btn.disabled = false;
            }).catch(err => {
                console.error("Error generating image:", err);
                alert("Terjadi kesalahan saat membuat gambar poster.");
                btn.innerHTML = originalText;
                btn.disabled = false;
            });
        }

        // Initialize application
        window.onload = () => {
            renderRosterInputs();
            updatePoster();
        };
    </script>
</body>
</html>
