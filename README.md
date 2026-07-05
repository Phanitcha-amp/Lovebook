<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Our Story - Happy Anniversary</title>
    
    <!-- ใช้ Tailwind CSS v3 ตัวเสถียรเพื่อป้องกันการเออร์เรอร์บนเบราว์เซอร์ -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Alpine.js สำหรับควบคุมการเปิดหน้าเว็บ -->
    <script defer src="https://cdn.jsdelivr.net/npm/alpinejs@3.x.x/dist/cdn.min.js"></script>
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Charm:wght@400;700&family=Kanit:wght@300;400;500&family=Playfair+Display:ital,wght@0,400..900;1,400..900&display=swap');
        
        .font-serif { font-family: 'Playfair Display', 'Kanit', serif; }
        .font-handwriting { font-family: 'Charm', cursive; }
        
        .perspective-2000 { perspective: 2000px; }
        .paper-texture {
            background-image: url('https://www.transparenttextures.com/patterns/lined-paper-2.png');
            background-blend-mode: multiply;
        }
        
        @keyframes floatUp {
            0% { transform: translateY(105vh) translateX(0) rotate(0deg); opacity: 0; }
            10% { opacity: 0.6; }
            90% { opacity: 0.6; }
            100% { transform: translateY(-10vh) translateX(50px) rotate(360deg); opacity: 0; }
        }
        .petal {
            position: absolute;
            background: rgba(244, 63, 94, 0.15);
            border-radius: 100% 0 100% 100%;
            pointer-events: none;
            z-index: 1;
        }
    </style>
</head>
<body class="min-h-screen w-full bg-gradient-to-br from-[#ece5da] via-[#e5dac9] to-[#d8cbba] flex flex-col items-center justify-center overflow-hidden relative font-serif"
      x-data="{ 
         isOpen: false, 
         currentPage: 1, 
         totalSpreads: 5,
         isMuted: true,
         finalStage: 0,
         audio: null,
         initAudio() {
             if(!this.audio) {
                 this.audio = new Audio('https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3');
                 this.audio.loop = true;
             }
         },
         toggleMute() {
             this.initAudio();
             if(this.isMuted) {
                 this.audio.play().catch(() => {});
             } else {
                 this.audio.pause();
             }
             this.isMuted = !this.isMuted;
         },
         openBook() {
             this.isOpen = true;
             this.currentPage = 1;
             this.initAudio();
             if(this.isMuted) { this.toggleMute(); }
         },
         resetBook() {
             this.isOpen = false;
             this.currentPage = 1;
             this.finalStage = 0;
         },
         nextPage() {
             if(this.currentPage < this.totalSpreads) {
                 this.currentPage++;
                 if(this.currentPage === 5) { this.triggerFinalTimeline(); }
             }
         },
         prevPage() {
              if(this.currentPage > 1) this.currentPage--;
         },
         triggerFinalTimeline() {
             this.finalStage = 0;
             setTimeout(() => { this.finalStage = 1; }, 1000);
             setTimeout(() => { this.finalStage = 2; }, 5000);
             setTimeout(() => { this.finalStage = 3; }, 7500);
         }
      }"
      x-init="
         for(let i=0; i<20; i++) {
             let petal = document.createElement('div');
             petal.className = 'petal';
             petal.style.width = (Math.random() * 8 + 6) + 'px';
             petal.style.height = (Math.random() * 8 + 6) + 'px';
             petal.style.left = (Math.random() * 100) + 'vw';
             petal.style.animation = 'floatUp ' + (Math.random() * 10 + 12) + 's linear infinite';
             petal.style.animationDelay = (Math.random() * 6) + 's';
             $el.appendChild(petal);
         }
      ">

    <!-- ปุ่มควบคุมเสียง -->
    <div class="absolute top-6 right-6 z-50">
        <button @click="toggleMute()" class="p-3 rounded-full bg-white/80 backdrop-blur-md shadow-md border border-stone-200 hover:bg-white text-stone-600 transition-all transform hover:scale-105">
            <template x-if="isMuted">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5"><path stroke-linecap="round" stroke-linejoin="round" d="M17.25 9.75 19.5 12m0 0 2.25 2.25M19.5 12l2.25-2.25M19.5 12l-2.25 2.25m-10.5-6 4.72-4.72a.75.75 0 0 1 1.28.53v15.88a.75.75 0 0 1-1.28.53l-4.72-4.72H4.51c-.88 0-1.704-.507-1.938-1.354A9.009 9.009 0 0 1 2.25 12c0-.83.112-1.633.322-2.396C2.806 8.756 3.63 8.25 4.51 8.25H6.75Z" /></svg>
            </template>
            <template x-if="!isMuted">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 animate-pulse"><path stroke-linecap="round" stroke-linejoin="round" d="M19.114 5.636a9 9 0 0 1 0 12.728M16.463 8.288a5.25 5.25 0 0 1 0 7.424M6.75 8.25l4.72-4.72a.75.75 0 0 1 1.28.53v15.88a.75.75 0 0 1-1.28.53l-4.72-4.72H4.51c-.88 0-1.704-.507-1.938-1.354A9.009 9.009 0 0 1 2.25 12c0-.83.112-1.633.322-2.396C2.806 8.756 3.63 8.25 4.51 8.25H6.75Z" /></svg>
            </template>
        </button>
    </div>

    <!-- ส่วนตัวเล่มสมุดความทรงจำ -->
    <main class="w-full max-w-5xl px-4 flex flex-col items-center justify-center z-10 perspective-2000 min-h-[580px]">
        
        <!-- หน้าปกเมื่อปิดอยู่ -->
        <div x-show="!isOpen" 
             @click="openBook()"
             class="w-[380px] h-[540px] bg-[#4a3329] rounded-r-2xl rounded-l-md shadow-[25px_25px_40px_rgba(0,0,0,0.35)] border-y border-r border-stone-950/40 cursor-pointer group flex flex-col justify-between p-8 relative overflow-hidden select-none">
            
            <div class="absolute left-0 top-0 bottom-0 w-4 bg-gradient-to-r from-black/40 via-transparent to-transparent pointer-events-none"></div>
            <div class="absolute inset-4 border border-amber-600/20 rounded-md pointer-events-none p-1">
                <div class="border border-dashed border-amber-600/10 h-full w-full rounded-sm"></div>
            </div>

            <div class="text-center mt-12 relative z-10 space-y-4">
                <div class="text-amber-500/80 text-4xl">♥</div>
                <h1 class="text-4xl font-bold tracking-widest text-[#f5efe6] uppercase">Our Story</h1>
                <h2 class="text-sm tracking-widest text-amber-400/80 font-sans font-semibold uppercase pt-2">Happy Anniversary</h2>
            </div>

            <div class="text-center relative z-10 px-4 mb-8">
                <p class="text-stone-400 font-serif italic text-xs leading-relaxed">
                    Every page holds a memory, and every memory holds a piece of us.
                </p>
                <span class="block text-[10px] uppercase tracking-widest text-amber-500/50 mt-6 group-hover:text-amber-400 transition-colors">
                    คลิกเพื่อเปิดสมุดบันทึก
                </span>
            </div>
        </div>

        <!-- เนื้อหาด้านในเมื่อเปิดสมุดออก -->
        <div x-show="isOpen" 
             class="w-full max-w-4xl h-[560px] bg-[#3d2a21] p-2.5 rounded-2xl shadow-[0_30px_60px_rgba(0,0,0,0.4)] flex relative select-none">
            
            <!-- ปุ่มเปลี่ยนหน้าซ้าย-ขวา -->
            <button x-show="currentPage > 1" @click="prevPage()" class="absolute left-4 top-1/2 -translate-y-1/2 z-30 p-2.5 rounded-full bg-white/90 border border-stone-200 shadow-md text-stone-600 hover:bg-white transition-all"><svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-5 h-5"><path stroke-linecap="round" stroke-linejoin="round" d="M15.75 19.5 8.25 12l7.5-7.5" /></svg></button>
            <button x-show="currentPage < totalSpreads" @click="nextPage()" class="absolute right-4 top-1/2 -translate-y-1/2 z-30 p-2.5 rounded-full bg-white/90 border border-stone-200 shadow-md text-stone-600 hover:bg-white transition-all"><svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-5 h-5"><path stroke-linecap="round" stroke-linejoin="round" d="m8.25 4.5 7.5 7.5-7.5 7.5" /></svg></button>

            <!-- หน้าซ้าย (Left Page) -->
            <div class="w-1/2 h-full bg-[#fdfbf7] rounded-l-md border-r border-stone-300/40 relative p-8 overflow-hidden flex flex-col justify-between paper-texture">
                <div>
                    <h3 class="text-xl font-bold tracking-wide text-stone-800 border-b border-stone-200 pb-2 mb-4">
                        <template x-if="currentPage === 1"><span>The Beginning</span></template>
                        <template x-if="currentPage === 2"><span>Our First Date</span></template>
                        <template x-if="currentPage === 3"><span>Our Adventures</span></template>
                        <template x-if="currentPage === 4"><span>My Safe Place</span></template>
                        <template x-if="currentPage === 5"><span>Forever Bound</span></template>
                    </h3>

                    <div class="text-stone-700 text-sm h-[400px] relative">
                        <!-- Spread 1: Left -->
                        <div x-show="currentPage === 1" class="space-y-4 pt-2">
                            <p class="text-stone-500 italic text-xs">วันแรกที่เราได้ทำความรู้จักกัน...</p>
                            <div class="border border-stone-200 bg-stone-50/60 rounded-xl p-4 shadow-inner space-y-3 relative">
                                <div class="absolute -top-2 left-12 w-16 h-4 bg-amber-100/60 border border-dashed border-amber-200 rotate-[-2deg]"></div>
                                <div class="flex gap-2 items-start text-xs">
                                    <div class="bg-rose-100 text-rose-700 p-1 rounded font-mono">20:14</div>
                                    <div class="bg-white rounded-lg p-2 text-stone-700 shadow-sm border border-stone-100 max-w-[80%]">
                                        เธอๆ เราเห็นเธอชอบมานั่งอ่านหนังสือที่ร้านกาแฟนี้บ่อยๆ เล่มนั้นสนุกไหมหรอ? 😊
                                    </div>
                                </div>
                                <div class="flex gap-2 items-start justify-end text-xs">
                                    <div class="bg-amber-100 rounded-lg p-2 text-stone-700 shadow-sm border border-amber-200 max-w-[80%]">
                                        อ๋อ! เล่มนี้สนุกมากเลยค่ะ วางไม่ลงเลย ยินดีที่ได้คุยด้วยนะคะดวงดาวคนใหม่
                                    </div>
                                    <div class="bg-stone-100 text-stone-500 p-1 rounded font-mono">20:18</div>
                                </div>
                            </div>
                            <div class="pt-4 border-t border-dashed border-stone-300">
                                <p class="font-handwriting text-rose-500 text-lg leading-relaxed">พิมพ์เสร็จมือนี่สั่นไปหมดเลยตอนนั้น ดีใจจังที่รวบรวมความกล้าทักไป</p>
                            </div>
                        </div>

                        <!-- Spread 2: Left -->
                        <div x-show="currentPage === 2" class="flex flex-col justify-between h-full py-2">
                            <div class="bg-white p-3 pb-8 shadow-md transform -rotate-2 border border-stone-200 relative">
                                <div class="absolute -top-3 left-6 w-20 h-5 bg-amber-100/70 shadow-sm"></div>
                                <div class="w-full h-44 bg-stone-200 rounded flex items-center justify-center text-stone-400 text-xs uppercase tracking-widest">
                                    [ รูปถ่ายเดทแรกของเรา ]
                                </div>
                                <p class="font-handwriting text-center text-stone-600 mt-3 text-lg">เดินเล่นในสวนพฤกษศาสตร์ด้วยกัน 🌿</p>
                            </div>
                            <div class="bg-stone-50/80 border border-stone-200 p-3 rounded-lg text-xs italic text-stone-500 mt-4">
                                "เราสองคนเดินคุยกันไปเรื่อยๆ จนลืมมองเวลา ดอกไม้รอบข้างสวยมาก แต่สายตาเรามองแต่เธอ"
                            </div>
                        </div>

                        <!-- Spread 3: Left -->
                        <div x-show="currentPage === 3" class="grid grid-cols-2 gap-3 pt-2">
                            <div class="bg-white p-2 shadow border border-stone-200 rounded transform -rotate-4 h-36 flex flex-col justify-between">
                                <div class="w-full h-24 bg-stone-100 rounded flex items-center justify-center text-[10px] text-stone-400">[ทริปภูเขา]</div>
                                <span class="font-handwriting text-xs text-center text-stone-500">อากาศหนาวๆ กับเธอ 🏔️</span>
                            </div>
                            <div class="bg-white p-2 shadow border border-stone-200 rounded transform rotate-3 h-36 flex flex-col justify-between mt-4">
                                <div class="w-full h-24 bg-stone-100 rounded flex items-center justify-center text-[10px] text-stone-400">[คาเฟ่เปิดใหม่]</div>
                                <span class="font-handwriting text-xs text-center text-stone-500">กาแฟแก้วโปรด ☕</span>
                            </div>
                            <div class="bg-white p-2 shadow border border-stone-200 rounded col-span-2 w-[90%] mx-auto mt-2 flex flex-col justify-between">
                                <div class="w-full h-20 bg-stone-100 rounded flex items-center justify-center text-[10px] text-stone-400">[รูปวิวสวยๆ]</div>
                                <span class="font-handwriting text-xs text-center text-stone-500 pt-1">ทุกการเดินทางมีความหมายเพราะมีเธอนะ</span>
                            </div>
                        </div>

                        <!-- Spread 4: Left -->
                        <div x-show="currentPage === 4" class="h-full flex flex-col justify-center items-center px-2">
                            <div class="bg-white p-3 pb-10 shadow-xl border border-stone-200 w-full transform -rotate-1 relative">
                                <div class="absolute -top-3 left-16 w-24 h-5 bg-amber-100/50 shadow-sm"></div>
                                <div class="w-full h-64 bg-stone-200 rounded flex items-center justify-center text-stone-400 text-xs uppercase tracking-widest">
                                    [ รูปโพลารอยด์คู่น่ารักๆ ]
                                </div>
                                <p class="font-handwriting text-center text-stone-600 mt-4 text-xl">My favorite smile.</p>
                            </div>
                        </div>

                        <!-- Spread 5: Left -->
                        <div x-show="currentPage === 5" class="h-full p-1">
                            <div class="bg-white p-3 pb-10 shadow-lg border border-stone-200 w-full h-full flex flex-col justify-between">
                                <div class="w-full h-[90%] bg-stone-200 rounded flex items-center justify-center text-stone-400 text-xs uppercase tracking-widest">
                                    [ รูปภาพคู่ฉลองวันครบรอบ ]
                                </div>
                                <div class="text-center font-handwriting text-stone-500 text-sm mt-2">ขอบคุณที่เติบโตไปด้วยกันในทุกๆ วันนะ</div>
                            </div>
                        </div>
                    </div>
                </div>
                <span class="absolute bottom-4 left-6 text-xs text-stone-400 font-mono" x-text="(currentPage * 2) - 1"></span>
            </div>

            <!-- สันสมุดตรงกลาง -->
            <div class="w-1.5 h-full bg-gradient-to-r from-stone-400/20 via-stone-900/50 to-stone-400/20 relative z-20"></div>

            <!-- หน้าขวา (Right Page) -->
            <div class="w-1/2 h-full bg-[#fdfbf7] rounded-r-md relative p-8 overflow-hidden flex flex-col justify-between paper-texture">
                <div>
                    <h3 class="text-xl font-bold tracking-wide text-stone-800 border-b border-stone-200 pb-2 mb-4">
                        <template x-if="currentPage === 1"><span>Where Everything Started</span></template>
                        <template x-if="currentPage === 2"><span>Our First Meal Together</span></template>
                        <template x-if="currentPage === 3"><span>The Fun Side of Us</span></template>
                        <template x-if="currentPage === 4"><span>Home</span></template>
                        <template x-if="currentPage === 5"><span>Our Continuation</span></template>
                    </h3>

                    <div class="text-stone-700 text-sm h-[400px] relative">
                        <!-- Spread 1: Right -->
                        <div x-show="currentPage === 1" class="flex flex-col h-full justify-between py-2">
                            <div class="space-y-4">
                                <div class="flex items-center space-x-3 bg-stone-50 p-2.5 rounded-lg border border-stone-100">
                                    <div class="text-amber-700 text-xl">☕</div>
                                    <div>
                                        <h4 class="text-[10px] font-bold text-stone-400 uppercase tracking-wider">The First Encounter</h4>
                                        <p class="text-xs font-medium">The Cozy Corner Cafe, โต๊ะริมหน้าต่าง</p>
                                    </div>
                                </div>
                                <div class="space-y-2">
                                    <span class="text-[11px] text-stone-400 block uppercase">First Impression ของฉัน:</span>
                                    <p class="text-xs italic text-stone-600 leading-relaxed font-serif">
                                        "เธอมีพลังงานบางอย่างที่ดึงดูดสายตามาก เวลามุ่งมั่นอ่านหนังสือแล้วเหมือนโลกทั้งใบหยุดนิ่งไปเลย น่ารักตั้งแต่แรกเห็นเลยนะ"
                                    </p>
                                </div>
                            </div>
                            <div class="text-center pt-6 border-t border-stone-200/60">
                                <p class="font-serif italic text-stone-500 text-xs">“It all started with one simple conversation.”</p>
                                <div class="mt-2 text-rose-300 text-xs">✦ ♥ ✦</div>
                            </div>
                        </div>

                        <!-- Spread 2: Right -->
                        <div x-show="currentPage === 2" class="flex flex-col justify-between h-full py-2">
                            <div class="bg-white p-3 pb-8 shadow-md transform rotate-3 border border-stone-200 relative">
                                <div class="absolute -top-3 right-6 w-16 h-5 bg-amber-100/60 shadow-sm"></div>
                                <div class="w-full h-36 bg-stone-200 rounded flex items-center justify-center text-stone-400 text-xs uppercase tracking-widest">
                                    [ รูปมื้ออาหารแรกของเรา ]
                                </div>
                                <p class="font-handwriting text-center text-stone-600 mt-2 text-lg">ร้านราเมงลับๆ ในตรอกเล็กๆ 🍜</p>
                            </div>
                            <div class="space-y-2 text-center pt-2">
                                <p class="font-handwriting text-stone-500 text-base">จำได้ว่าตอนนั้นแย่งกันกินหมูชาชูชิ้นสุดท้ายด้วย สรุปเธอสละให้เรากินเฉยเลย ใจดีจัง!</p>
                                <p class="font-serif italic text-stone-600 text-xs border-t border-stone-100 pt-2">
                                    “I never imagined that one meal would become one of my favorite memories.”
                                </p>
                            </div>
                        </div>

                        <!-- Spread 3: Right -->
                        <div x-show="currentPage === 3" class="relative h-full flex flex-col justify-between py-1">
                            <div class="absolute top-0 right-2 text-amber-500/20 font-handwriting text-4xl select-none rotate-12">5555+</div>
                            <div class="grid grid-cols-2 gap-2 mt-4">
                                <div class="bg-white p-1.5 shadow border border-stone-200 rounded transform rotate-6">
                                    <div class="w-full h-20 bg-stone-100 rounded flex items-center justify-center text-[10px] text-stone-400">[เซลฟี่หน้าตลก]</div>
                                </div>
                                <div class="bg-white p-1.5 shadow border border-stone-200 rounded transform -rotate-5">
                                    <div class="w-full h-20 bg-stone-100 rounded flex items-center justify-center text-[10px] text-stone-400">[หลุดๆ เผลอๆ]</div>
                                </div>
                            </div>
                            <div class="font-handwriting text-stone-600 text-base space-y-1 bg-amber-50/60 p-2 rounded border border-amber-200/40 mt-4">
                                <p>• วีรกรรมตอนทำเค้กไหม้จนสโมคดีเทคเตอร์ดัง 🤣</p>
                                <p>• บทสนทนาตีสามเรื่องมนุษย์ต่างดาวมีจริงไหม</p>
                            </div>
                        </div>

                        <!-- Spread 4: Right -->
                        <div x-show="currentPage === 4" class="h-full flex flex-col justify-center items-center px-2">
                            <div class="bg-white p-3 pb-10 shadow-xl border border-stone-200 w-full transform rotate-2 relative">
                                <div class="absolute -top-3 right-16 w-20 h-5 bg-amber-100/50 shadow-sm"></div>
                                <div class="w-full h-64 bg-stone-200 rounded flex items-center justify-center text-stone-400 text-xs uppercase tracking-widest">
                                    [ รูปคู่ที่ชอบที่สุดอีกรูป ]
                                </div>
                                <p class="font-handwriting text-center text-stone-600 mt-4 text-xl">My safe place. My favorite person.</p>
                            </div>
                        </div>

                        <!-- Spread 5: Right (หน้าจดหมายซึ้งๆ ค่อยๆ ขึ้น) -->
                        <div x-show="currentPage === 5" class="h-full flex flex-col justify-between py-1 text-center">
                            <div class="flex-grow flex flex-col justify-center items-center px-2">
                                <div class="space-y-3 text-stone-800 transition-opacity duration-[2000ms]"
                                     :class="finalStage >= 1 ? 'opacity-100' : 'opacity-0'">
                                    <p class="font-bold tracking-wide text-sm">ขอให้เรื่องราวของเราไม่มีวันจบ</p>
                                    <p class="text-xs">และมีเรื่องราวให้เราได้ขีดเขียนลงในสมุดเล่มนี้ไปเรื่อย ๆ</p>
                                    <p class="text-xs">ขอบคุณที่อยู่เคียงข้างกันในทุกช่วงเวลาของชีวิต</p>
                                    <p class="text-rose-600 font-medium font-handwriting text-xl pt-1">สุขสันต์วันครบรอบนะ 🤍</p>
                                </div>

                                <div class="h-14 mt-6 flex items-center justify-center">
                                    <p class="font-handwriting text-stone-400 text-xl italic transition-opacity duration-1000"
                                       x-show="finalStage === 2">
                                        The End?
                                    </p>
                                    <div class="transition-all duration-1000 transform" x-show="finalStage >= 3">
                                        <p class="font-handwriting text-stone-500 text-sm">No...</p>
                                        <p class="italic text-stone-600 text-xs tracking-wider">Just another beautiful chapter.</p>
                                    </div>
                                </div>
                            </div>

                            <div class="pt-4 border-t border-stone-200/60 transition-opacity duration-700"
                                 :class="finalStage >= 3 ? 'opacity-100' : 'opacity-0 pointer-events-none'">
                                <button @click="resetBook()" class="inline-flex items-center space-x-2 px-5 py-2 rounded-full bg-stone-900 text-stone-50 hover:bg-stone-800 text-xs tracking-widest uppercase transition-all transform hover:scale-105 shadow-md">
                                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-3.5 h-3.5"><path stroke-linecap="round" stroke-linejoin="round" d="M16.023 9.348h4.992v-.001M2.985 19.644v-4.992m0 0h4.992m-4.993 0 3.181 3.183a8.25 8.25 0 0 0 13.803-3.7M4.031 9.865a8.25 8.25 0 0 1 13.803-3.7l3.181 3.182m0-4.991v4.99" /></svg>
                                    <span>Read Again</span>
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
                <span class="absolute bottom-4 right-6 text-xs text-stone-400 font-mono" x-text="currentPage * 2"></span>
            </div>
        </div>
    </main>

</body>
</html>
