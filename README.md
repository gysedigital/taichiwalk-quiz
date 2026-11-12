import React, { useState, useEffect } from 'react';
import confetti from 'canvas-confetti';
import { X, Check, ArrowRight, ShieldCheck, Sparkles, HeartHandshake, Award } from 'lucide-react';

// Brand Logo (Ultra-compressed Taichi Walk Emblem WebP)
import taichiWalkLogo from '../assets/images/taichi_walk_logo_opt.webp';

// Welcome Screen Radiant Image (Optimized 63KB WebP)
import welcomeSunroomImg from '../assets/images/latina_sunroom_welcome_opt.webp';

// Hyperrealistic Authentic Latina Women (All ultra-compressed 16KB - 35KB WebP)
import latinaWalkingParkImg from '../assets/images/latina_walking_park_opt.webp';
import latinaAbdomenReliefImg from '../assets/images/latina_abdomen_relief_opt.webp';
import latinaKneeReliefImg from '../assets/images/latina_knee_relief_opt.webp';
import latinaPeacefulSleepImg from '../assets/images/latina_peaceful_sleep_opt.webp';
import latinaTaichiLivingroomImg from '../assets/images/latina_taichi_livingroom_opt.webp';
import latinaHerbalInfusionImg from '../assets/images/latina_herbal_infusion_opt.webp';
import latinaBackReliefImg from '../assets/images/latina_back_relief_opt.webp';
import latinaHabitJournalImg from '../assets/images/latina_habit_journal_opt.webp';

const CHECKOUT_URL = 'https://gydigimarket.impultienda.ar/checkout';

interface ModuleDetail {
  title: string;
  subtitle: string;
  icon: string;
  image: string;
  badge?: string;
  description: string;
  features: string[];
}

const MODULE_DETAILS: Record<string, ModuleDetail> = {
  core: {
    title: 'Programa Tai Chi Walk (8 Semanas)',
    subtitle: 'Guía interactiva y plan metabólico paso a paso',
    icon: '🧘‍♀️',
    image: latinaTaichiLivingroomImg,
    description: 'El método de caminata consciente y bajo impacto diseñado especialmente para mujeres reales de 38 a 55 años que desean reactivar su metabolismo sin forzar articulaciones ni articulaciones cansadas.',
    features: [
      'Rutinas de 15 a 20 minutos de bajo impacto para hacer en casa',
      'Paso a paso guiado con respiración armónica y postura erguida',
      'Guía digital interactiva descargable en PDF lista para celular o imprimir',
      'Enfoque especial en activar la circulación y quema de grasa profunda',
    ],
  },
  bono1: {
    title: 'Bono 1: Calentamiento Articular',
    subtitle: 'Rutina ilustrada de 5 minutos',
    icon: '🌱',
    image: latinaKneeReliefImg,
    description: 'Secuencia suave de movilidad articular para lubricar rodillas, caderas y columna antes de cada caminata, aliviando la sensación de rigidez matutina.',
    features: [
      'Solo 5 minutos antes de iniciar tu caminata',
      'Libera tensión acumulada en zona lumbar, caderas y piernas',
      'Totalmente libre de saltos, posturas complejas o esfuerzo desmedido',
    ],
  },
  bono2: {
    title: 'Bono 2: 45 Infusiones Desinflamatorias',
    subtitle: 'Recetas digestivas, detox y anti-estrés',
    icon: '🍵',
    image: latinaHerbalInfusionImg,
    description: 'Recetario exclusivo de tés e infusiones con ingredientes naturales cotidianos para desinflamar el abdomen, acelerar el drenaje y mejorar la digestión.',
    features: [
      '45 combinaciones con hierbas y especias naturales fáciles de conseguir',
      'Infusiones para tomar en ayunas, después de comer y antes de dormir',
      'Propiedades diuréticas, desinflamantes y digestivas comprobadas',
    ],
  },
  bono3: {
    title: 'Bono 3: Tracker de Hábitos 8 Semanas',
    subtitle: 'Plantilla de seguimiento visual imprimible',
    icon: '📅',
    image: latinaHabitJournalImg,
    description: 'Lleva el registro de tus días de caminata, nivel de energía, hidratación y descanso con una plantilla motivacional diseñada para mantenerte enfocada con calma.',
    features: [
      'Formato imprimible de alta resolución y digital editable',
      'Seguimiento semanal de bienestar, medidas y vitalidad diaria',
      'Frases inspiradoras para cada semana de avance',
    ],
  },
  bump: {
    title: 'Recuperación Articular Express (3 Min)',
    subtitle: 'Estiramiento suave post-caminata',
    icon: '✨',
    image: latinaBackReliefImg,
    badge: 'INCLUIDO',
    description: 'Mini-rutina de 3 minutos al finalizar la caminata para devolver la elasticidad a tus músculos, relajar tendones y sentir alivio y ligereza inmediata en piernas y espalda.',
    features: [
      'Estiramientos suaves de piernas, cadera y zona lumbar',
      'Evita dolores posteriores, contracturas o calambres',
      'Sensación de ligereza y piernas descansadas al instante',
    ],
  },
};

interface MiniAppFlowProps {
  onSwitchToFullQuiz?: () => void;
}

export const MiniAppFlow: React.FC<MiniAppFlowProps> = ({ onSwitchToFullQuiz }) => {
  // Screen 0: Welcome Screen (Pantalla de Bienvenida)
  // Screens 1-4: Quiz questions
  // Screen 5: Loading / Analysis
  // Screen 6: Dashboard & Checkout
  const [currentScreen, setCurrentScreen] = useState<number>(0);
  const [userSelections, setUserSelections] = useState<{
    1?: string;
    2?: string;
    3?: string;
    4?: string;
  }>({});
  const [selectedModule, setSelectedModule] = useState<ModuleDetail | null>(null);

  useEffect(() => {
    // Meta Pixel PageView
    try {
      if (typeof window !== 'undefined' && (window as unknown as { fbq?: (...args: unknown[]) => void }).fbq) {
        (window as unknown as { fbq: (...args: unknown[]) => void }).fbq('track', 'PageView');
      }
    } catch {
      // ignore
    }
  }, []);

  const handleSelectOption = (step: number, value: string) => {
    setUserSelections((prev) => ({ ...prev, [step]: value }));
  };

  const handleNext = (targetStep: number) => {
    if (targetStep === 5) {
      setCurrentScreen(5);
      // Simulate analysis for 2.5s
      setTimeout(() => {
        setCurrentScreen(6); // Dashboard
        try {
          confetti({
            particleCount: 80,
            spread: 70,
            origin: { y: 0.6 },
            colors: ['#d94f4f', '#38a169', '#f6ad55', '#4299e1'],
          });
        } catch {
          // ignore
        }

        // Track ViewContent on reaching the plan
        try {
          if (typeof window !== 'undefined' && (window as unknown as { fbq?: (...args: unknown[]) => void }).fbq) {
            (window as unknown as { fbq: (...args: unknown[]) => void }).fbq('track', 'ViewContent', {
              content_name: 'Tai Chi Walk 8 Semanas - MiniApp',
              content_category: 'Fitness & Health Plan',
              value: 11900,
              currency: 'ARS',
            });
          }
        } catch {
          // ignore
        }
      }, 2500);
    } else {
      setCurrentScreen(targetStep);
      window.scrollTo({ top: 0, behavior: 'smooth' });
    }
  };

  const handleCheckoutRedirect = () => {
    // Meta Pixel InitiateCheckout tracking
    try {
      if (typeof window !== 'undefined' && (window as unknown as { fbq?: (...args: unknown[]) => void }).fbq) {
        (window as unknown as { fbq: (...args: unknown[]) => void }).fbq('track', 'InitiateCheckout', {
          content_name: 'Tai Chi Walk 8 Semanas',
          content_category: 'Fitness & Health Plan',
          value: 11900,
          currency: 'ARS',
        });
      }
    } catch {
      // ignore
    }

    try {
      if (window.top && window.top !== window) {
        window.top.location.href = CHECKOUT_URL;
        return;
      }
    } catch {
      // cross-origin fallback
    }
    window.location.href = CHECKOUT_URL;
  };

  const handleRestart = () => {
    setUserSelections({});
    setCurrentScreen(0);
    window.scrollTo({ top: 0, behavior: 'smooth' });
  };

  // Determine assigned level title based on time selected
  const getTimeLevelInfo = () => {
    const timeChoice = userSelections[4];
    if (timeChoice === '10') {
      return {
        levelName: 'Nivel 1 (Inicio Suave y Articulaciones)',
        desc: 'Enfocado en 10-15 min de caminata armónica con activación articular.',
      };
    } else if (timeChoice === '30') {
      return {
        levelName: 'Nivel 3 (Progresión y Quema Metabólica)',
        desc: 'Enfocado en 30 min para acelerar la quema de grasa sin impacto.',
      };
    }
    return {
      levelName: 'Nivel 2 (Construcción y Flujo)',
      desc: 'Enfocado en 20-25 min equilibrando movilidad, tonificación y calma.',
    };
  };

  const levelInfo = getTimeLevelInfo();

  return (
    <div className="min-h-screen bg-[#faf8f5] text-[#2c2c2c] flex flex-col justify-center items-center p-3 sm:p-5">
      {/* Main Container Card */}
      <div className="w-full max-w-[480px] bg-white rounded-2xl shadow-[0_10px_30px_rgba(0,0,0,0.06)] overflow-hidden flex flex-col min-h-[680px] relative border border-[#e6e2dc]/70">
        
        {/* Brand Header with Logo TAICHI WALK (Visible on ALL screens) */}
        <div className="px-5 pt-3.5 pb-2.5 flex items-center justify-between border-b border-stone-100 bg-[#fffdfb] sticky top-0 z-20">
          <div className="flex items-center gap-2.5 select-none">
            <img
              src={taichiWalkLogo}
              alt="Tai Chi Walk"
              width={32}
              height={32}
              loading="eager"
              decoding="async"
              referrerPolicy="no-referrer"
              className="w-8 h-8 rounded-lg object-cover shadow-2xs border border-stone-200/90 shrink-0"
            />
            <div className="flex flex-col">
              <span className="text-[14px] sm:text-[15px] font-black tracking-wider uppercase text-[#1a1a1a] leading-tight">
                TAICHI WALK
              </span>
              <span className="text-[10px] font-bold text-[#d94f4f] leading-none tracking-wide">
                PLAN PERSONALIZADO
              </span>
            </div>
          </div>

          {currentScreen === 0 && (
            <span className="text-[11px] font-bold text-stone-600 bg-stone-100 px-2.5 py-1 rounded-full border border-stone-200">
              Método Consciente
            </span>
          )}

          {currentScreen >= 1 && currentScreen <= 4 && (
            <span className="text-xs font-bold text-[#6b6b6b] uppercase tracking-wider bg-stone-100 px-2.5 py-1 rounded-full border border-stone-200">
              Paso {currentScreen} de 5
            </span>
          )}

          {currentScreen === 5 && (
            <span className="text-xs font-bold text-[#d94f4f] bg-red-50 px-2.5 py-1 rounded-full border border-red-200 animate-pulse">
              Analizando...
            </span>
          )}

          {currentScreen === 6 && (
            <span className="text-[11px] font-extrabold text-emerald-700 bg-emerald-50 px-2.5 py-1 rounded-full border border-emerald-200 flex items-center gap-1">
              <Award className="w-3.5 h-3.5" /> Plan Asignado
            </span>
          )}
        </div>

        {/* Progress Bar (Visible in steps 1 to 4) */}
        {currentScreen >= 1 && currentScreen <= 4 && (
          <div className="w-full h-1.5 bg-[#e6e2dc] overflow-hidden">
            <div
              className="h-full bg-[#d94f4f] transition-all duration-300 ease-out"
              style={{ width: `${(currentScreen / 5) * 100}%` }}
            />
          </div>
        )}

        {/* ========================================================================= */}
        {/* PANTALLA DE BIENVENIDA (Screen 0) */}
        {/* ========================================================================= */}
        {currentScreen === 0 && (
          <div className="p-5 sm:p-6 flex flex-col flex-grow justify-between animate-fadeIn">
            <div>
              {/* Luminous Sunroom Hero Image (63KB, instant eager load) */}
              <div className="w-full h-56 sm:h-64 rounded-2xl overflow-hidden bg-stone-100 mb-4 border border-[#e6e2dc] shadow-sm relative group">
                <img
                  src={welcomeSunroomImg}
                  alt="Mujer en galería luminosa practicando Tai Chi caminando"
                  width={640}
                  height={480}
                  loading="eager"
                  fetchPriority="high"
                  decoding="async"
                  referrerPolicy="no-referrer"
                  className="w-full h-full object-cover object-top transition-transform duration-300 group-hover:scale-102"
                />
                <div className="absolute top-3 left-3 bg-white/95 backdrop-blur-xs text-[#1a1a1a] text-[11px] font-bold px-2.5 py-1 rounded-full shadow-xs border border-stone-200/80 flex items-center gap-1.5">
                  <span className="w-2 h-2 rounded-full bg-emerald-500 animate-pulse" />
                  Bajo Impacto Articular
                </div>
              </div>

              {/* Main Headline */}
              <h1 className="text-[22px] sm:text-[24px] font-extrabold text-[#1a1a1a] text-center leading-tight mb-2 tracking-tight">
                ¡Comienza tu viaje de Tai Chi caminando hoy!
              </h1>

              {/* Subtitle */}
              <p className="text-[13.5px] sm:text-[14px] text-stone-600 text-center leading-relaxed px-1 sm:px-2 mb-4">
                Descubre el método de caminar Tai Chi diseñado para mujeres - una manera suave y efectiva de perder peso, aumentar tu energía y reconectar con la alegría de sentirse increíble en tu cuerpo.
              </p>

              {/* Micro Benefits Pills */}
              <div className="grid grid-cols-3 gap-2 mb-4 text-center">
                <div className="bg-[#FAF8F5] p-2 rounded-xl border border-stone-200/80">
                  <span className="text-base block mb-0.5">🌿</span>
                  <span className="text-[11px] font-bold text-stone-800 block leading-tight">Cero Impacto</span>
                  <span className="text-[10px] text-stone-500">Cuida rodillas</span>
                </div>
                <div className="bg-[#FAF8F5] p-2 rounded-xl border border-stone-200/80">
                  <span className="text-base block mb-0.5">✨</span>
                  <span className="text-[11px] font-bold text-stone-800 block leading-tight">15-20 Min</span>
                  <span className="text-[10px] text-stone-500">En tu sala</span>
                </div>
                <div className="bg-[#FAF8F5] p-2 rounded-xl border border-stone-200/80">
                  <span className="text-base block mb-0.5">🌸</span>
                  <span className="text-[11px] font-bold text-stone-800 block leading-tight">+40 Años</span>
                  <span className="text-[10px] text-stone-500">Mujeres reales</span>
                </div>
              </div>
            </div>

            {/* Main Action Button */}
            <button
              id="btn-welcome-continue"
              onClick={() => handleNext(1)}
              className="w-full bg-[#d94f4f] hover:bg-[#c23f3f] active:scale-[0.99] text-white p-4 rounded-[14px] text-base font-extrabold cursor-pointer transition-all text-center shadow-md flex items-center justify-center gap-2 uppercase tracking-wide"
            >
              <span>CONTINUAR</span>
              <ArrowRight className="w-5 h-5 stroke-[2.5]" />
            </button>
          </div>
        )}

        {/* ========================================================================= */}
        {/* PASO 1: Objetivo Principal */}
        {/* ========================================================================= */}
        {currentScreen === 1 && (
          <div className="p-5 sm:p-6 flex flex-col flex-grow justify-between animate-fadeIn">
            <div>
              <h2 className="text-[21px] sm:text-[22px] font-bold text-[#1a1a1a] text-center mb-1 leading-tight">
                ¿Qué querés lograr con tu rutina?
              </h2>
              <p className="text-xs sm:text-sm text-[#6b6b6b] text-center mb-5">
                Selecciona tu objetivo principal
              </p>

              <div className="flex flex-col gap-2.5 mb-5">
                {[
                  {
                    id: 'peso',
                    label: 'Perder peso de forma sostenible',
                    desc: 'Caminata suave y quema calórica sin impacto',
                    image: latinaWalkingParkImg,
                  },
                  {
                    id: 'abdomen',
                    label: 'Reducir grasa abdominal e hinchazón',
                    desc: 'Desinflamación y cintura más liviana',
                    image: latinaAbdomenReliefImg,
                  },
                  {
                    id: 'energia',
                    label: 'Tonificar el cuerpo y recuperar energía',
                    desc: 'Movimientos armónicos que renuevan tu vitalidad',
                    image: latinaTaichiLivingroomImg,
                  },
                  {
                    id: 'rigidez',
                    label: 'Aliviar rigidez y moverme sin dolor',
                    desc: 'Articulaciones flexibles y pasos suaves y seguros',
                    image: latinaKneeReliefImg,
                  },
                ].map((option) => {
                  const isSelected = userSelections[1] === option.id;
                  return (
                    <div
                      key={option.id}
                      onClick={() => handleSelectOption(1, option.id)}
                      className={`flex items-center justify-between p-2.5 sm:p-3 rounded-xl border-2 cursor-pointer transition-all ${
                        isSelected
                          ? 'border-[#d94f4f] bg-[#fff5f5] shadow-xs'
                          : 'border-[#e6e2dc] bg-white hover:border-[#d94f4f] hover:bg-[#fdfbfb]'
                      }`}
                    >
                      <div className="flex items-center gap-3 flex-1 pr-2">
                        {/* Radio Circle */}
                        <div
                          className={`w-5 h-5 rounded-full border-2 flex items-center justify-center shrink-0 transition-colors ${
                            isSelected
                              ? 'border-[#d94f4f] bg-[#d94f4f]'
                              : 'border-[#e6e2dc] bg-white'
                          }`}
                        >
                          {isSelected && <div className="w-2 h-2 bg-white rounded-full" />}
                        </div>
                        <div className="flex flex-col">
                          <span className="text-[14px] sm:text-[15px] font-bold text-[#1a1a1a] leading-snug">
                            {option.label}
                          </span>
                          <span className="text-[11px] text-[#6b6b6b] leading-tight mt-0.5">
                            {option.desc}
                          </span>
                        </div>
                      </div>

                      {/* Visual Avatar / Ultra-compressed WebP with lazy loading */}
                      <div className="w-14 h-14 sm:w-15 sm:h-15 rounded-xl overflow-hidden bg-[#faf8f5] shrink-0 border border-stone-200 shadow-2xs">
                        <img
                          src={option.image}
                          alt={option.label}
                          width={60}
                          height={60}
                          loading="lazy"
                          decoding="async"
                          referrerPolicy="no-referrer"
                          className="w-full h-full object-cover object-center transition-transform duration-200 hover:scale-105"
                        />
                      </div>
                    </div>
                  );
                })}
              </div>
            </div>

            <button
              disabled={!userSelections[1]}
              onClick={() => handleNext(2)}
              className="w-full bg-[#d94f4f] hover:bg-[#c23f3f] disabled:bg-[#dcd6d0] text-white p-3.5 sm:p-4 rounded-[14px] text-base font-bold cursor-pointer disabled:cursor-not-allowed transition-colors text-center shadow-sm"
            >
              Continuar
            </button>
          </div>
        )}

        {/* ========================================================================= */}
        {/* PASO 2: Zona de incomodidad */}
        {/* ========================================================================= */}
        {currentScreen === 2 && (
          <div className="p-5 sm:p-6 flex flex-col flex-grow justify-between animate-fadeIn">
            <div>
              <h2 className="text-[21px] sm:text-[22px] font-bold text-[#1a1a1a] text-center mb-1 leading-tight">
                ¿Cuál es la zona que más te incomoda?
              </h2>
              <p className="text-xs sm:text-sm text-[#6b6b6b] text-center mb-5">
                Para personalizar tus ejercicios de Tai Chi Walk
              </p>

              <div className="flex flex-col gap-2.5 mb-5">
                {[
                  {
                    id: 'rodillas',
                    label: 'Rodillas y piernas cansadas',
                    desc: 'Sensibilidad al dar pasos largos o subir escaleras',
                    image: latinaKneeReliefImg,
                  },
                  {
                    id: 'vientre',
                    label: 'Vientre inflamado al final del día',
                    desc: 'Pesadez y retención abdominal acumulada',
                    image: latinaAbdomenReliefImg,
                  },
                  {
                    id: 'lumbar',
                    label: 'Espalda baja / zona lumbar',
                    desc: 'Tensión postural al permanecer parada o sentada',
                    image: latinaBackReliefImg,
                  },
                  {
                    id: 'general',
                    label: 'Pesadez general en todo el cuerpo',
                    desc: 'Hombros cargados y falta de liviandad muscular',
                    image: latinaTaichiLivingroomImg,
                  },
                ].map((option) => {
                  const isSelected = userSelections[2] === option.id;
                  return (
                    <div
                      key={option.id}
                      onClick={() => handleSelectOption(2, option.id)}
                      className={`flex items-center justify-between p-2.5 sm:p-3 rounded-xl border-2 cursor-pointer transition-all ${
                        isSelected
                          ? 'border-[#d94f4f] bg-[#fff5f5] shadow-xs'
                          : 'border-[#e6e2dc] bg-white hover:border-[#d94f4f] hover:bg-[#fdfbfb]'
                      }`}
                    >
                      <div className="flex items-center gap-3 flex-1 pr-2">
                        {/* Radio Circle */}
                        <div
                          className={`w-5 h-5 rounded-full border-2 flex items-center justify-center shrink-0 transition-colors ${
                            isSelected
                              ? 'border-[#d94f4f] bg-[#d94f4f]'
                              : 'border-[#e6e2dc] bg-white'
                          }`}
                        >
                          {isSelected && <div className="w-2 h-2 bg-white rounded-full" />}
                        </div>
                        <div className="flex flex-col">
                          <span className="text-[14px] sm:text-[15px] font-bold text-[#1a1a1a] leading-snug">
                            {option.label}
                          </span>
                          <span className="text-[11px] text-[#6b6b6b] leading-tight mt-0.5">
                            {option.desc}
                          </span>
                        </div>
                      </div>

                      {/* Visual Avatar / Ultra-compressed WebP with lazy loading */}
                      <div className="w-14 h-14 sm:w-15 sm:h-15 rounded-xl overflow-hidden bg-[#faf8f5] shrink-0 border border-stone-200 shadow-2xs">
                        <img
                          src={option.image}
                          alt={option.label}
                          width={60}
                          height={60}
                          loading="lazy"
                          decoding="async"
                          referrerPolicy="no-referrer"
                          className="w-full h-full object-cover object-center transition-transform duration-200 hover:scale-105"
                        />
                      </div>
                    </div>
                  );
                })}
              </div>
            </div>

            <button
              disabled={!userSelections[2]}
              onClick={() => handleNext(3)}
              className="w-full bg-[#d94f4f] hover:bg-[#c23f3f] disabled:bg-[#dcd6d0] text-white p-3.5 sm:p-4 rounded-[14px] text-base font-bold cursor-pointer disabled:cursor-not-allowed transition-colors text-center shadow-sm"
            >
              Continuar
            </button>
          </div>
        )}

        {/* ========================================================================= */}
        {/* PASO 3: Sueño / Estilo de vida */}
        {/* ========================================================================= */}
        {currentScreen === 3 && (
          <div className="p-5 sm:p-6 flex flex-col flex-grow justify-between animate-fadeIn">
            <div>
              <h2 className="text-[21px] sm:text-[22px] font-bold text-[#1a1a1a] text-center mb-1 leading-tight">
                ¿Cómo impacta el cansancio en tu rutina?
              </h2>
              <p className="text-xs sm:text-sm text-[#6b6b6b] text-center mb-5">
                Queremos entender tu nivel de energía diario
              </p>

              <div className="flex flex-col gap-2.5 mb-5">
                {[
                  {
                    id: 'sueno',
                    label: 'Me cuesta conciliar el sueño o descanso mal',
                    desc: 'Despertar sin energía o sueño interrumpido',
                    image: latinaPeacefulSleepImg,
                  },
                  {
                    id: 'estres',
                    label: 'Siento mucho estrés o ansiedad acumulada',
                    desc: 'Mente acelerada y dificultad para desconectar',
                    image: latinaHerbalInfusionImg,
                  },
                  {
                    id: 'sedentario',
                    label: 'Llevo una vida muy sedentaria / ajetreada',
                    desc: 'Poco tiempo libre y muchas horas de tensión',
                    image: latinaHabitJournalImg,
                  },
                  {
                    id: 'bien',
                    label: 'Duermo bien pero busco más vitalidad',
                    desc: 'Deseo mayor ligereza, tono y energía cotidiana',
                    image: latinaWalkingParkImg,
                  },
                ].map((option) => {
                  const isSelected = userSelections[3] === option.id;
                  return (
                    <div
                      key={option.id}
                      onClick={() => handleSelectOption(3, option.id)}
                      className={`flex items-center justify-between p-2.5 sm:p-3 rounded-xl border-2 cursor-pointer transition-all ${
                        isSelected
                          ? 'border-[#d94f4f] bg-[#fff5f5] shadow-xs'
                          : 'border-[#e6e2dc] bg-white hover:border-[#d94f4f] hover:bg-[#fdfbfb]'
                      }`}
                    >
                      <div className="flex items-center gap-3 flex-1 pr-2">
                        {/* Radio Circle */}
                        <div
                          className={`w-5 h-5 rounded-full border-2 flex items-center justify-center shrink-0 transition-colors ${
                            isSelected
                              ? 'border-[#d94f4f] bg-[#d94f4f]'
                              : 'border-[#e6e2dc] bg-white'
                          }`}
                        >
                          {isSelected && <div className="w-2 h-2 bg-white rounded-full" />}
                        </div>
                        <div className="flex flex-col">
                          <span className="text-[14px] sm:text-[15px] font-bold text-[#1a1a1a] leading-snug">
                            {option.label}
                          </span>
                          <span className="text-[11px] text-[#6b6b6b] leading-tight mt-0.5">
                            {option.desc}
                          </span>
                        </div>
                      </div>

                      {/* Visual Avatar / Ultra-compressed WebP with lazy loading */}
                      <div className="w-14 h-14 sm:w-15 sm:h-15 rounded-xl overflow-hidden bg-[#faf8f5] shrink-0 border border-stone-200 shadow-2xs">
                        <img
                          src={option.image}
                          alt={option.label}
                          width={60}
                          height={60}
                          loading="lazy"
                          decoding="async"
                          referrerPolicy="no-referrer"
                          className="w-full h-full object-cover object-center transition-transform duration-200 hover:scale-105"
                        />
                      </div>
                    </div>
                  );
                })}
              </div>
            </div>

            <button
              disabled={!userSelections[3]}
              onClick={() => handleNext(4)}
              className="w-full bg-[#d94f4f] hover:bg-[#c23f3f] disabled:bg-[#dcd6d0] text-white p-3.5 sm:p-4 rounded-[14px] text-base font-bold cursor-pointer disabled:cursor-not-allowed transition-colors text-center shadow-sm"
            >
              Continuar
            </button>
          </div>
        )}

        {/* ========================================================================= */}
        {/* PASO 4: Tiempo disponible */}
        {/* ========================================================================= */}
        {currentScreen === 4 && (
          <div className="p-5 sm:p-6 flex flex-col flex-grow justify-between animate-fadeIn">
            <div>
              <h2 className="text-[21px] sm:text-[22px] font-bold text-[#1a1a1a] text-center mb-1 leading-tight">
                ¿Cuánto tiempo podés dedicarle al día?
              </h2>
              <p className="text-xs sm:text-sm text-[#6b6b6b] text-center mb-5">
                Diseñado para adaptarse a tu ritmo sin exigirte
              </p>

              <div className="flex flex-col gap-2.5 mb-5">
                {[
                  {
                    id: '10',
                    label: '10 a 15 minutos',
                    badge: 'Inicio Suave - Nivel 1',
                    desc: 'Activación articular y pasos lentos sin cansancio',
                    image: latinaKneeReliefImg,
                  },
                  {
                    id: '20',
                    label: '20 a 25 minutos',
                    badge: 'Construcción - Nivel 2',
                    desc: 'Flujo continuo para quemar calorías y calmar la mente',
                    image: latinaTaichiLivingroomImg,
                  },
                  {
                    id: '30',
                    label: '30 minutos o más',
                    badge: 'Progresión - Nivel 3',
                    desc: 'Estimulación metabólica profunda y tonificación',
                    image: latinaWalkingParkImg,
                  },
                ].map((option) => {
                  const isSelected = userSelections[4] === option.id;
                  return (
                    <div
                      key={option.id}
                      onClick={() => handleSelectOption(4, option.id)}
                      className={`flex items-center justify-between p-2.5 sm:p-3 rounded-xl border-2 cursor-pointer transition-all ${
                        isSelected
                          ? 'border-[#d94f4f] bg-[#fff5f5] shadow-xs'
                          : 'border-[#e6e2dc] bg-white hover:border-[#d94f4f] hover:bg-[#fdfbfb]'
                      }`}
                    >
                      <div className="flex items-center gap-3 flex-1 pr-2">
                        {/* Radio Circle */}
                        <div
                          className={`w-5 h-5 rounded-full border-2 flex items-center justify-center shrink-0 transition-colors ${
                            isSelected
                              ? 'border-[#d94f4f] bg-[#d94f4f]'
                              : 'border-[#e6e2dc] bg-white'
                          }`}
                        >
                          {isSelected && <div className="w-2 h-2 bg-white rounded-full" />}
                        </div>
                        <div className="flex flex-col">
                          <div className="flex items-center gap-2">
                            <span className="text-[14px] sm:text-[15px] font-bold text-[#1a1a1a]">
                              {option.label}
                            </span>
                            <span className="text-[10px] font-bold text-[#d94f4f] bg-red-100/60 px-1.5 py-0.5 rounded">
                              {option.badge}
                            </span>
                          </div>
                          <span className="text-[11px] text-[#6b6b6b] leading-tight mt-0.5">
                            {option.desc}
                          </span>
                        </div>
                      </div>

                      {/* Visual Avatar / Ultra-compressed WebP with lazy loading */}
                      <div className="w-14 h-14 sm:w-15 sm:h-15 rounded-xl overflow-hidden bg-[#faf8f5] shrink-0 border border-stone-200 shadow-2xs">
                        <img
                          src={option.image}
                          alt={option.label}
                          width={60}
                          height={60}
                          loading="lazy"
                          decoding="async"
                          referrerPolicy="no-referrer"
                          className="w-full h-full object-cover object-center transition-transform duration-200 hover:scale-105"
                        />
                      </div>
                    </div>
                  );
                })}
              </div>
            </div>

            <button
              disabled={!userSelections[4]}
              onClick={() => handleNext(5)}
              className="w-full bg-[#d94f4f] hover:bg-[#c23f3f] disabled:bg-[#dcd6d0] text-white p-3.5 sm:p-4 rounded-[14px] text-base font-bold cursor-pointer disabled:cursor-not-allowed transition-colors text-center shadow-sm"
            >
              Continuar
            </button>
          </div>
        )}

        {/* ========================================================================= */}
        {/* PASO 5: Pantalla de carga simulando análisis */}
        {/* ========================================================================= */}
        {currentScreen === 5 && (
          <div className="p-8 flex flex-col flex-grow justify-center items-center text-center animate-fadeIn gap-5">
            <div className="relative">
              <div className="w-16 h-16 border-4 border-[#e6e2dc] border-t-[#d94f4f] rounded-full animate-spin" />
              <div className="absolute inset-0 flex items-center justify-center">
                <img
                  src={taichiWalkLogo}
                  alt="Tai Chi Walk"
                  width={32}
                  height={32}
                  loading="eager"
                  decoding="async"
                  className="w-8 h-8 rounded-full object-cover opacity-90 animate-pulse"
                />
              </div>
            </div>

            <div>
              <h2 className="text-[22px] font-bold text-[#1a1a1a] mb-2">
                Analizando tus respuestas...
              </h2>
              <p className="text-sm text-[#6b6b6b] max-w-xs mx-auto leading-relaxed">
                Estamos configurando tu Plan Personalizado de Tai Chi Walk y seleccionando tus bonos exclusivos.
              </p>
            </div>

            <div className="w-full max-w-xs bg-stone-100 rounded-full h-2 overflow-hidden mt-2">
              <div className="h-full bg-gradient-to-r from-[#d94f4f] to-[#e53e3e] w-4/5 animate-pulse rounded-full" />
            </div>
            <span className="text-xs font-semibold text-stone-500">
              Generando protocolo para {levelInfo.levelName.split(' ')[0]} {levelInfo.levelName.split(' ')[1]}...
            </span>
          </div>
        )}

        {/* ========================================================================= */}
        {/* RESULTADO / DASHBOARD DE LA MINIAPP */}
        {/* ========================================================================= */}
        {currentScreen === 6 && (
          <div className="overflow-y-auto max-h-[720px] p-4 sm:p-5 pb-8 animate-fadeIn">
            {/* Header Box */}
            <div className="bg-gradient-to-br from-[#fff5f5] to-white border border-[#fed7d7] p-4 sm:p-5 rounded-[14px] mb-5 text-center shadow-sm">
              <div className="inline-flex items-center gap-1.5 px-3 py-1 rounded-full bg-red-100/70 text-[#d94f4f] text-xs font-extrabold uppercase tracking-wide mb-2">
                <Sparkles className="w-3.5 h-3.5" /> Plan Personalizado Asignado
              </div>
              <h3 className="text-[#1a1a1a] text-[18px] sm:text-[20px] font-black mb-1">
                ¡Tu Plan de Tai Chi Walk está listo!
              </h3>
              <p className="text-[13px] text-[#4a4a4a] leading-snug">
                Nivel Asignado: <strong className="text-[#d94f4f]">{levelInfo.levelName}</strong> + 3 Bonos Exclusivos
              </p>
              <p className="text-[11px] text-stone-500 mt-1 italic">
                {levelInfo.desc}
              </p>
            </div>

            {/* Core del Programa */}
            <div className="mb-5">
              <h4 className="text-[13px] font-bold text-[#444] mb-2.5 uppercase tracking-wider flex items-center justify-between">
                <span>Core del Programa</span>
                <span className="text-[10px] text-[#d94f4f] font-bold bg-red-50 px-2 py-0.5 rounded border border-red-200">
                  Base Principal
                </span>
              </h4>
              <button
                type="button"
                onClick={() => setSelectedModule(MODULE_DETAILS.core)}
                className="w-full bg-white border border-[#e6e2dc] hover:border-[#d94f4f] rounded-xl p-3 sm:px-3.5 flex items-center justify-between text-left transition-all hover:-translate-y-0.5 shadow-sm group cursor-pointer"
              >
                <div className="flex items-center gap-3 flex-1 min-w-0 pr-2">
                  {/* Visual Illustration Icon */}
                  <div className="w-13 h-13 sm:w-14 sm:h-14 rounded-xl overflow-hidden bg-[#fff5f5] shrink-0 border border-stone-200 shadow-2xs relative">
                    <img
                      src={MODULE_DETAILS.core.image}
                      alt={MODULE_DETAILS.core.title}
                      width={56}
                      height={56}
                      loading="lazy"
                      decoding="async"
                      referrerPolicy="no-referrer"
                      className="w-full h-full object-cover object-center group-hover:scale-105 transition-transform duration-200"
                    />
                    <div className="absolute bottom-0 right-0 bg-white/90 rounded-tl-md px-1 text-[11px]">
                      {MODULE_DETAILS.core.icon}
                    </div>
                  </div>
                  <div className="flex-1 min-w-0">
                    <div className="text-[14px] font-bold text-[#222] group-hover:text-[#d94f4f] transition-colors truncate">
                      {MODULE_DETAILS.core.title}
                    </div>
                    <div className="text-[12px] text-[#6b6b6b] line-clamp-1">
                      {MODULE_DETAILS.core.subtitle}
                    </div>
                  </div>
                </div>
                <span className="text-[#38a169] font-bold text-[16px] ml-2 shrink-0 bg-emerald-50 w-7 h-7 rounded-full flex items-center justify-center border border-emerald-200">
                  ✓
                </span>
              </button>
            </div>

            {/* Bonos de Regalo Incluidos */}
            <div className="mb-5">
              <h4 className="text-[13px] font-bold text-[#444] mb-2.5 uppercase tracking-wider flex items-center justify-between">
                <span>Bonos de Regalo Incluidos</span>
                <span className="text-[10px] text-emerald-700 bg-emerald-50 px-2 py-0.5 rounded border border-emerald-200 font-bold">
                  100% Bonificados
                </span>
              </h4>

              <div className="flex flex-col gap-2">
                {[
                  { key: 'bono1', data: MODULE_DETAILS.bono1 },
                  { key: 'bono2', data: MODULE_DETAILS.bono2 },
                  { key: 'bono3', data: MODULE_DETAILS.bono3 },
                ].map(({ key, data }) => (
                  <button
                    key={key}
                    type="button"
                    onClick={() => setSelectedModule(data)}
                    className="w-full bg-white border border-[#e6e2dc] hover:border-[#d94f4f] rounded-xl p-3 sm:px-3.5 flex items-center justify-between text-left transition-all hover:-translate-y-0.5 shadow-sm group cursor-pointer"
                  >
                    <div className="flex items-center gap-3 flex-1 min-w-0 pr-2">
                      {/* Visual Illustration Icon */}
                      <div className="w-13 h-13 sm:w-14 sm:h-14 rounded-xl overflow-hidden bg-[#fff5f5] shrink-0 border border-stone-200 shadow-2xs relative">
                        <img
                          src={data.image}
                          alt={data.title}
                          width={56}
                          height={56}
                          loading="lazy"
                          decoding="async"
                          referrerPolicy="no-referrer"
                          className="w-full h-full object-cover object-center group-hover:scale-105 transition-transform duration-200"
                        />
                        <div className="absolute bottom-0 right-0 bg-white/90 rounded-tl-md px-1 text-[11px]">
                          {data.icon}
                        </div>
                      </div>
                      <div className="flex-1 min-w-0">
                        <div className="text-[14px] font-bold text-[#222] group-hover:text-[#d94f4f] transition-colors truncate">
                          {data.title}
                        </div>
                        <div className="text-[12px] text-[#6b6b6b] line-clamp-1">
                          {data.subtitle}
                        </div>
                      </div>
                    </div>
                    <span className="text-[#38a169] font-bold text-[16px] ml-2 shrink-0 bg-emerald-50 w-7 h-7 rounded-full flex items-center justify-center border border-emerald-200">
                      ✓
                    </span>
                  </button>
                ))}
              </div>
            </div>

            {/* Order Bump / Personalización */}
            <div className="mb-5">
              <h4 className="text-[13px] font-bold text-[#444] mb-2.5 uppercase tracking-wider">
                Order Bump / Personalización
              </h4>
              <button
                type="button"
                onClick={() => setSelectedModule(MODULE_DETAILS.bump)}
                className="w-full bg-[#fffcfc] border border-[#d94f4f] rounded-xl p-3 sm:px-3.5 flex items-center justify-between text-left transition-all hover:-translate-y-0.5 shadow-sm group cursor-pointer"
              >
                <div className="flex items-center gap-3 flex-1 min-w-0 pr-2">
                  {/* Visual Illustration Icon */}
                  <div className="w-13 h-13 sm:w-14 sm:h-14 rounded-xl overflow-hidden bg-[#fff5f5] shrink-0 border border-[#fed7d7] shadow-2xs relative">
                    <img
                      src={MODULE_DETAILS.bump.image}
                      alt={MODULE_DETAILS.bump.title}
                      width={56}
                      height={56}
                      loading="lazy"
                      decoding="async"
                      referrerPolicy="no-referrer"
                      className="w-full h-full object-cover object-center group-hover:scale-105 transition-transform duration-200"
                    />
                    <div className="absolute bottom-0 right-0 bg-white/90 rounded-tl-md px-1 text-[11px]">
                      {MODULE_DETAILS.bump.icon}
                    </div>
                  </div>
                  <div className="flex-1 min-w-0">
                    <div className="text-[14px] font-bold text-[#222] truncate">
                      {MODULE_DETAILS.bump.title}
                    </div>
                    <div className="text-[12px] text-[#6b6b6b] line-clamp-1">
                      {MODULE_DETAILS.bump.subtitle}
                    </div>
                  </div>
                </div>
                <span className="text-[#d94f4f] font-black text-[12px] tracking-wide ml-2 shrink-0 bg-red-100/60 px-2 py-1 rounded">
                  INCLUIDO
                </span>
              </button>
            </div>

            {/* Checkout Box */}
            <div className="bg-[#1a202c] text-white p-5 rounded-[14px] text-center mt-4 shadow-lg border border-slate-800">
              <span className="inline-block text-[10px] font-extrabold text-[#fed7d7] bg-[#e54b4b]/20 border border-[#e54b4b]/40 px-2.5 py-0.5 rounded-full uppercase tracking-wider mb-2">
                🚀 Precio Especial de Lanzamiento
              </span>
              <p className="text-[13px] text-[#a0aec0] font-medium">
                Acceso Inmediato a la MiniApp + Materiales
              </p>

              {/* Price Display with Launch Price & Savings */}
              <div className="my-2.5">
                <div className="flex items-center justify-center gap-2 mb-1 flex-wrap">
                  <span className="text-xs text-slate-400 line-through">
                    $22.900 ARS
                  </span>
                  <span className="text-[11px] font-bold text-emerald-400 bg-emerald-950/60 px-2 py-0.5 rounded border border-emerald-800/60">
                    Ahorrás $11.000 ARS (48% OFF)
                  </span>
                </div>
                <div className="text-[32px] font-black text-[#48bb78] tracking-tight flex items-baseline justify-center gap-1.5 leading-tight">
                  <span>$11.900</span>
                  <span className="text-sm font-bold text-slate-300">ARS</span>
                </div>
                <p className="text-xs font-semibold text-slate-200 mt-1">
                  Pago único · Acceso vitalicio inmediato
                </p>
              </div>

              <p className="text-[11px] text-[#cbd5e0] mb-3.5">
                Sin mensualidades ni costos ocultos · Garantía de 7 días
              </p>

              {/* Tokens preservados para integración interna de ImpulTienda */}
              <span
                className="hidden"
                aria-hidden="true"
                data-price="11900"
                data-currency="ARS"
                data-original-price="22900"
                data-savings="11000"
              />

              <a
                href={CHECKOUT_URL}
                data-checkout
                id="btn-mercadopago"
                onClick={(e) => {
                  handleCheckoutRedirect();
                }}
                className="bg-[#009ee3] hover:bg-[#0082be] active:scale-[0.99] text-white w-full border-none py-3.5 px-4 rounded-xl font-black text-[15px] sm:text-base cursor-pointer transition-all shadow-md flex items-center justify-center gap-2 no-underline text-center select-none uppercase tracking-wide"
              >
                <span>ACCEDER A MI PLAN</span>
              </a>
              
              <div className="mt-3 flex items-center justify-center gap-3 text-[11px] text-slate-400">
                <span className="flex items-center gap-1">
                  <ShieldCheck className="w-3.5 h-3.5 text-emerald-400" /> Compra 100% Segura
                </span>
                <span>•</span>
                <span className="flex items-center gap-1">
                  <HeartHandshake className="w-3.5 h-3.5 text-sky-400" /> Entrega Digital Inmediata
                </span>
              </div>
            </div>

            {/* Secondary action: Restart */}
            <div className="mt-6 flex flex-col items-center gap-2 text-center">
              <button
                onClick={handleRestart}
                className="text-xs text-stone-500 hover:text-stone-800 underline transition cursor-pointer"
              >
                ← Reiniciar respuestas del cuestionario
              </button>
            </div>
          </div>
        )}
      </div>

      {/* Module Detail Modal with Full Illustration Preview */}
      {selectedModule && (
        <div className="fixed inset-0 z-50 bg-black/60 backdrop-blur-xs flex items-center justify-center p-4 animate-fadeIn">
          <div className="bg-white rounded-2xl max-w-sm w-full p-5 shadow-2xl border border-stone-200 relative animate-scaleUp">
            <button
              onClick={() => setSelectedModule(null)}
              className="absolute top-3.5 right-3.5 w-8 h-8 rounded-full bg-stone-100 text-stone-500 hover:text-stone-900 flex items-center justify-center cursor-pointer transition"
            >
              <X className="w-4 h-4" />
            </button>

            {/* Realistic Illustration preview in modal */}
            <div className="w-full h-40 rounded-xl overflow-hidden bg-stone-100 mb-3 border border-stone-200/80 shadow-2xs">
              <img
                src={selectedModule.image}
                alt={selectedModule.title}
                width={320}
                height={160}
                loading="eager"
                decoding="async"
                referrerPolicy="no-referrer"
                className="w-full h-full object-cover object-center"
              />
            </div>

            <div className="flex items-center gap-2.5 mb-2">
              <span className="text-xl">{selectedModule.icon}</span>
              <div>
                <h4 className="text-sm font-bold text-stone-900 leading-tight">
                  {selectedModule.title}
                </h4>
                <p className="text-xs text-[#d94f4f] font-semibold">
                  {selectedModule.subtitle}
                </p>
              </div>
            </div>

            <p className="text-xs text-stone-600 mb-3 leading-relaxed">
              {selectedModule.description}
            </p>

            <div className="bg-stone-50 rounded-xl p-3 border border-stone-200/80 mb-4 space-y-1.5">
              <span className="text-[11px] font-bold text-stone-700 uppercase tracking-wider block">
                ¿Qué incluye este material?
              </span>
              {selectedModule.features.map((feature, idx) => (
                <div key={idx} className="flex items-start gap-2 text-xs text-stone-700">
                  <Check className="w-3.5 h-3.5 text-emerald-600 shrink-0 mt-0.5" />
                  <span>{feature}</span>
                </div>
              ))}
            </div>

            <a
              href={CHECKOUT_URL}
              data-checkout
              onClick={() => {
                setSelectedModule(null);
                handleCheckoutRedirect();
              }}
              className="w-full py-3 bg-[#d94f4f] hover:bg-[#c23f3f] text-white font-bold text-xs uppercase tracking-wider rounded-xl transition flex items-center justify-center gap-1.5 cursor-pointer shadow-md no-underline text-center"
            >
              <span>ACCEDER A MI PLAN ($11.900 ARS)</span>
              <ArrowRight className="w-4 h-4" />
            </a>
          </div>
        </div>
      )}
    </div>
  );
};

