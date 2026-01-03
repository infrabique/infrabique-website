import React, { useState, useEffect } from 'react';
import { motion, AnimatePresence } from 'framer-motion';
import { 
  Building2, 
  Ruler, 
  FileCheck, 
  ShieldCheck, 
  Activity, 
  ChevronRight, 
  Menu, 
  X, 
  BarChart3, 
  Layers, 
  MapPin, 
  CheckCircle2,
  ArrowRight,
  Mail,
  Phone,
  Grid,
  Linkedin,
  Twitter,
  Instagram,
  Facebook,
  Youtube,
  Briefcase,
  AlertCircle,
  Clock,
  Sun,
  Moon,
  Upload // Changed to Upload for better compatibility
} from 'lucide-react';

/**
 * INFRABIQUE SERVICES
 * Enterprise Infrastructure & Technical Consultancy Website
 * * Fixed: Duplicate component definitions and icon compatibility
 */

// REPLACE THIS URL WITH THE PATH TO YOUR UPLOADED LOGO
const LOGO_URL = "https://placehold.co/200x200/ea580c/ffffff.png?text=LOGO";

// --- Utility Functions ---
const scrollToSection = (id) => {
  const element = document.getElementById(id);
  if (element) {
    element.scrollIntoView({ behavior: 'smooth' });
  }
};

// --- Components ---

// 0. Live Clock Component (Defined ONCE)
const LiveClock = ({ theme }) => {
  const [time, setTime] = useState(new Date());

  useEffect(() => {
    const timer = setInterval(() => setTime(new Date()), 1000);
    return () => clearInterval(timer);
  }, []);

  return (
    <div className={`flex items-center gap-2 text-xs font-mono ${theme === 'dark' ? 'text-stone-400' : 'text-stone-600'}`}>
      <Clock size={14} className="text-orange-600" />
      <span className="tracking-widest">
        {time.toLocaleTimeString([], { hour12: true, hour: '2-digit', minute: '2-digit', second: '2-digit' }).toUpperCase()}
      </span>
    </div>
  );
};

// 1. Job Application Modal Component
const ApplicationModal = ({ isOpen, onClose, jobRole, theme }) => {
  if (!isOpen) return null;

  const [formData, setFormData] = useState({
    name: '',
    email: '',
    phone: '',
    message: ''
  });
  const [fileName, setFileName] = useState('');

  const handleInputChange = (e) => {
    setFormData({ ...formData, [e.target.name]: e.target.value });
  };

  const handleFileChange = (e) => {
    if (e.target.files.length > 0) {
      setFileName(e.target.files[0].name);
    }
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    const subject = `Job Application: ${jobRole} - ${formData.name}`;
    const body = `Name: ${formData.name}%0D%0AEmail: ${formData.email}%0D%0APhone: ${formData.phone}%0D%0A%0D%0ACover Letter / Message:%0D%0A${formData.message}%0D%0A%0D%0A[IMPORTANT: Please attach your resume (${fileName || 'file'}) to this email manually.]`;
    
    window.location.href = `mailto:info@infrabique.com?subject=${encodeURIComponent(subject)}&body=${body}`;
    onClose();
  };

  const bgClass = theme === 'dark' ? 'bg-stone-900 border-stone-700 text-white' : 'bg-white border-stone-200 text-stone-900';
  const inputClass = theme === 'dark' ? 'bg-stone-800 border-stone-600 text-white focus:border-orange-500' : 'bg-stone-50 border-stone-300 text-stone-900 focus:border-orange-500';

  return (
    <div className="fixed inset-0 z-[60] flex items-center justify-center p-4 bg-black/60 backdrop-blur-sm">
      <motion.div 
        initial={{ opacity: 0, scale: 0.95 }}
        animate={{ opacity: 1, scale: 1 }}
        exit={{ opacity: 0, scale: 0.95 }}
        className={`w-full max-w-lg rounded-xl shadow-2xl overflow-hidden border ${bgClass}`}
      >
        <div className="flex justify-between items-center p-6 border-b border-stone-200 dark:border-stone-700">
          <div>
            <h3 className="text-xl font-bold">Apply for Position</h3>
            <p className="text-sm text-orange-600 font-medium">{jobRole}</p>
          </div>
          <button onClick={onClose} className="p-1 rounded-full hover:bg-stone-200 dark:hover:bg-stone-700 transition-colors">
            <X size={20} />
          </button>
        </div>

        <form onSubmit={handleSubmit} className="p-6 space-y-4">
          <div>
            <label className="block text-xs font-bold uppercase mb-1 opacity-70">Full Name</label>
            <input type="text" name="name" required value={formData.name} onChange={handleInputChange} className={`w-full p-3 rounded-lg border outline-none transition-colors ${inputClass}`} placeholder="John Doe" />
          </div>
          <div className="grid grid-cols-2 gap-4">
            <div>
              <label className="block text-xs font-bold uppercase mb-1 opacity-70">Email</label>
              <input type="email" name="email" required value={formData.email} onChange={handleInputChange} className={`w-full p-3 rounded-lg border outline-none transition-colors ${inputClass}`} placeholder="john@example.com" />
            </div>
            <div>
              <label className="block text-xs font-bold uppercase mb-1 opacity-70">Phone</label>
              <input type="tel" name="phone" required value={formData.phone} onChange={handleInputChange} className={`w-full p-3 rounded-lg border outline-none transition-colors ${inputClass}`} placeholder="+91 98765 43210" />
            </div>
          </div>
          <div>
            <label className="block text-xs font-bold uppercase mb-1 opacity-70">Cover Letter</label>
            <textarea name="message" rows="3" value={formData.message} onChange={handleInputChange} className={`w-full p-3 rounded-lg border outline-none transition-colors resize-none ${inputClass}`} placeholder="Briefly describe your experience..."></textarea>
          </div>
          <div>
            <label className="block text-xs font-bold uppercase mb-1 opacity-70">Resume / CV</label>
            <div className={`relative border-2 border-dashed rounded-lg p-4 text-center cursor-pointer transition-colors ${theme === 'dark' ? 'border-stone-600 hover:border-orange-500 hover:bg-stone-800' : 'border-stone-300 hover:border-orange-500 hover:bg-orange-50'}`}>
              <input type="file" accept=".pdf,.doc,.docx" onChange={handleFileChange} className="absolute inset-0 w-full h-full opacity-0 cursor-pointer" />
              <div className="flex flex-col items-center gap-2 pointer-events-none">
                <Upload size={24} className="text-orange-500" />
                <span className="text-sm font-medium">{fileName || "Click to attach Resume"}</span>
              </div>
            </div>
          </div>
          <div className="pt-2">
            <button type="submit" className="w-full bg-orange-600 hover:bg-orange-700 text-white font-bold py-3 rounded-lg transition-all shadow-lg">Draft Application Email</button>
          </div>
        </form>
      </motion.div>
    </div>
  );
};

// 2. Intro Animation (Blueprint/Tech Load)
const IntroOverlay = ({ onComplete }) => {
  useEffect(() => {
    const timer = setTimeout(() => {
      onComplete();
    }, 3200); 
    return () => clearTimeout(timer);
  }, [onComplete]);

  return (
    <motion.div
      className="fixed inset-0 z-50 flex flex-col items-center justify-center bg-stone-950 text-white overflow-hidden"
      initial={{ opacity: 1 }}
      exit={{ opacity: 0, transition: { duration: 0.8, ease: "easeInOut" } }}
    >
      <div className="absolute inset-0 opacity-20 pointer-events-none">
        <div className="absolute inset-0" 
             style={{ 
               backgroundImage: 'linear-gradient(#ea580c 1px, transparent 1px), linear-gradient(90deg, #ea580c 1px, transparent 1px)', 
               backgroundSize: '40px 40px' 
             }}>
        </div>
      </div>
      <div className="relative z-10 flex flex-col items-center max-w-2xl px-6 text-center">
        <motion.div
          initial={{ scale: 0.8, opacity: 0 }}
          animate={{ scale: 1, opacity: 1 }}
          transition={{ duration: 0.8 }}
          className="mb-6 p-2 border border-orange-500/30 rounded-full bg-orange-900/20 backdrop-blur-sm"
        >
          <img src={LOGO_URL} alt="Infrabique Logo" className="w-24 h-24 object-contain rounded-full" />
        </motion.div>
        <motion.h1 
          className="text-3xl md:text-5xl font-bold tracking-tighter text-white mb-2"
          initial={{ y: 20, opacity: 0 }}
          animate={{ y: 0, opacity: 1 }}
          transition={{ delay: 0.4, duration: 0.6 }}
        >
          <span className="text-yellow-500">INFRABIQUE</span> <span className="text-orange-600">SERVICES</span>
        </motion.h1>
        <div className="mt-8 w-64 h-1 bg-stone-800 rounded-full overflow-hidden">
          <motion.div 
            className="h-full bg-gradient-to-r from-orange-600 to-lime-600"
            initial={{ width: "0%" }}
            animate={{ width: "100%" }}
            transition={{ duration: 2.5, ease: "easeInOut" }}
          />
        </div>
        <motion.div 
          className="mt-2 text-xs text-orange-500/80 font-mono"
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          transition={{ delay: 1.5 }}
        >
          INITIALIZING SYSTEM AUDIT...
        </motion.div>
      </div>
    </motion.div>
  );
};

const Navbar = ({ scrolled, theme, toggleTheme }) => {
  const [isOpen, setIsOpen] = useState(false);
  const navLinks = [
    { name: 'Services', href: 'services' },
    { name: 'About', href: 'about' },
    { name: 'Industries', href: 'industries' },
    { name: 'Careers', href: 'careers' },
    { name: 'Contact', href: 'contact' },
  ];

  const navBgClass = scrolled 
    ? (theme === 'dark' ? 'bg-stone-950/90 border-stone-800' : 'bg-white/95 border-stone-200') 
    : 'bg-transparent border-transparent';
  
  const linkColor = theme === 'dark' 
    ? 'text-yellow-400 hover:text-yellow-300' 
    : 'text-yellow-700 hover:text-yellow-800';

  return (
    <nav className={`fixed w-full z-40 transition-all duration-300 backdrop-blur-md shadow-sm border-b ${navBgClass} py-3`}>
      <div className="max-w-7xl mx-auto px-6 flex justify-between items-center">
        <div className="flex items-center gap-3 cursor-pointer" onClick={() => window.scrollTo({ top: 0, behavior: 'smooth' })}>
          <img src={LOGO_URL} alt="Infrabique Logo" className="w-10 h-10 object-contain rounded shadow-sm bg-white" />
          <span className={`text-xl font-bold tracking-tight ${theme === 'light' ? 'text-yellow-600' : 'text-yellow-400'}`}>INFRABIQUE</span>
        </div>

        <div className="hidden md:flex items-center gap-8">
          {navLinks.map((link) => (
            <button key={link.name} onClick={() => scrollToSection(link.href)} className={`text-sm font-medium transition-colors uppercase tracking-wide ${linkColor}`}>
              {link.name}
            </button>
          ))}
          <button onClick={toggleTheme} className={`p-2 rounded-full transition-colors ${theme === 'dark' ? 'bg-stone-800 text-yellow-400 hover:bg-stone-700' : 'bg-stone-100 text-yellow-700 hover:bg-stone-200'}`} title="Toggle Theme">
            {theme === 'dark' ? <Sun size={18} /> : <Moon size={18} />}
          </button>
          <motion.button whileTap={{ scale: 0.95 }} onClick={() => scrollToSection('contact')} className="bg-orange-600 hover:bg-orange-700 text-white px-5 py-2 text-sm font-medium rounded transition-colors flex items-center gap-2 shadow-lg">
            Consult Now <ChevronRight size={14} />
          </motion.button>
        </div>

        <div className="flex items-center gap-4 md:hidden">
           <button onClick={toggleTheme} className={`p-2 rounded-full transition-colors ${theme === 'dark' ? 'bg-stone-800 text-yellow-400' : 'bg-stone-100 text-yellow-700'}`}>
            {theme === 'dark' ? <Sun size={18} /> : <Moon size={18} />}
          </button>
          <motion.button whileTap={{ scale: 0.9 }} className={`${theme === 'dark' ? 'text-yellow-400' : 'text-yellow-700'}`} onClick={() => setIsOpen(!isOpen)}>
            {isOpen ? <X /> : <Menu />}
          </motion.button>
        </div>
      </div>

      <AnimatePresence>
        {isOpen && (
          <motion.div 
            initial={{ height: 0, opacity: 0 }}
            animate={{ height: 'auto', opacity: 1 }}
            exit={{ height: 0, opacity: 0 }}
            className={`md:hidden border-b overflow-hidden shadow-xl ${theme === 'dark' ? 'bg-stone-900 border-stone-800' : 'bg-white border-stone-200'}`}
          >
            <div className="flex flex-col p-6 gap-4">
              {navLinks.map((link) => (
                <button key={link.name} onClick={() => { scrollToSection(link.href); setIsOpen(false); }} className={`text-left font-medium ${theme === 'dark' ? 'text-yellow-400 hover:text-yellow-300' : 'text-yellow-700 hover:text-yellow-800'}`}>
                  {link.name}
                </button>
              ))}
            </div>
          </motion.div>
        )}
      </AnimatePresence>
    </nav>
  );
};

const Hero = ({ theme }) => {
  const bgClass = theme === 'dark' ? 'bg-stone-950' : 'bg-stone-100'; 
  const textClass = theme === 'dark' ? 'text-stone-400' : 'text-stone-600';
  const headingClass = theme === 'dark' ? 'text-white' : 'text-stone-900';
  const gradientOverlay = theme === 'dark' ? 'from-stone-950 via-stone-950/80' : 'from-stone-100 via-stone-100/80';
  const cardBg = theme === 'dark' ? 'bg-stone-900/90 border-stone-700' : 'bg-white/90 border-stone-200';
  const cardText = theme === 'dark' ? 'text-stone-500' : 'text-stone-500';
  const tickerBg = theme === 'dark' ? 'bg-stone-900 border-stone-800' : 'bg-white border-stone-200';

  return (
    <section className={`relative min-h-[750px] flex flex-col justify-center overflow-hidden pt-20 transition-colors duration-500 ${bgClass}`}>
      <div className="absolute inset-0 z-0">
         <img src="https://images.unsplash.com/photo-1451187580459-43490279c0fa?q=80&w=2072&auto=format&fit=crop" alt="Technical Grid" className="w-full h-full object-cover opacity-20" />
         <div className={`absolute inset-0 bg-gradient-to-t ${gradientOverlay} to-transparent`} />
         {theme === 'light' && (
            <div className="absolute top-0 right-0 w-full h-full opacity-100 pointer-events-none" style={{ background: 'radial-gradient(circle at top right, rgba(255, 237, 213, 0.5), transparent)' }}></div>
         )}
         <motion.div 
             className={`absolute inset-0 ${theme === 'dark' ? 'opacity-10' : 'opacity-20'}`}
             animate={{ backgroundPosition: ["0px 0px", "50px 50px"] }}
             transition={{ duration: 4, repeat: Infinity, ease: "linear" }}
             style={{ 
               backgroundImage: `linear-gradient(${theme === 'dark' ? '#ea580c' : '#a8a29e'} 1px, transparent 1px), linear-gradient(90deg, ${theme === 'dark' ? '#ea580c' : '#a8a29e'} 1px, transparent 1px)`, 
               backgroundSize: '50px 50px' 
             }}
          />
      </div>

      <div className="relative z-10 max-w-7xl mx-auto px-6 grid md:grid-cols-2 gap-12 items-center flex-grow py-12">
        <motion.div initial={{ x: -50, opacity: 0 }} animate={{ x: 0, opacity: 1 }} transition={{ duration: 0.8, delay: 0.5 }}>
          <div className="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-lime-900/40 border border-lime-800 text-lime-600 dark:text-lime-400 text-xs font-semibold uppercase tracking-wider mb-6">
            <motion.div animate={{ opacity: [1, 0.5, 1] }} transition={{ duration: 2, repeat: Infinity }} className="w-2 h-2 rounded-full bg-lime-600" />
            Technical Intelligence Live
          </div>
          <h1 className={`text-4xl md:text-6xl lg:text-7xl font-bold leading-tight mb-6 transition-colors duration-500 ${headingClass}`}>
            Engineering <br/> <span className="text-transparent bg-clip-text bg-gradient-to-r from-orange-500 to-lime-500">Confidence.</span>
          </h1>
          <p className={`text-lg md:text-xl mb-8 max-w-lg leading-relaxed transition-colors duration-500 ${textClass}`}>
            We deliver execution-ready infrastructure auditing, design, and verification services for enterprise-grade projects. Reduce risk. Optimize cost.
          </p>
          <div className="flex flex-col sm:flex-row gap-4">
            <motion.button whileTap={{ scale: 0.95 }} onClick={() => scrollToSection('contact')} className="bg-orange-600 hover:bg-orange-700 text-white px-8 py-3.5 rounded font-semibold transition-all shadow-lg">Request Consultation</motion.button>
            <motion.button whileTap={{ scale: 0.95 }} onClick={() => scrollToSection('services')} className={`bg-transparent border ${theme === 'dark' ? 'border-stone-600 text-stone-300 hover:border-white hover:text-white' : 'border-stone-400 text-stone-700 hover:border-stone-900 hover:text-stone-900'} px-8 py-3.5 rounded font-medium transition-all`}>View Services</motion.button>
          </div>
        </motion.div>

        <motion.div initial={{ opacity: 0, scale: 0.9 }} animate={{ opacity: 1, scale: 1 }} transition={{ duration: 1, delay: 0.8 }} className="hidden md:block relative h-[500px] w-full">
            <div className={`absolute inset-0 rounded-xl overflow-hidden border backdrop-blur-sm transition-colors duration-500 ${theme === 'dark' ? 'bg-stone-900/50 border-stone-800/50' : 'bg-white/50 border-stone-200'}`}>
                <img src="https://images.unsplash.com/photo-1503387762-592deb58ef4e?q=80&w=1931&auto=format&fit=crop" className="w-full h-full object-cover opacity-60 grayscale" alt="Architecture" />
                <motion.div className="absolute top-0 left-0 w-full h-1 bg-orange-500/80 shadow-[0_0_15px_rgba(249,115,22,0.8)]" animate={{ top: ["0%", "100%", "0%"] }} transition={{ duration: 6, repeat: Infinity, ease: "linear" }} />
                <motion.div className="absolute top-1/4 left-1/4 w-3 h-3 bg-lime-500 rounded-full shadow-[0_0_10px_#84cc16]" animate={{ scale: [1, 1.5, 1], opacity: [0.5, 1, 0.5] }} transition={{ duration: 2, repeat: Infinity }} />
                <motion.div className="absolute top-2/3 right-1/3 w-2 h-2 bg-orange-500 rounded-full shadow-[0_0_10px_#f97316]" animate={{ scale: [1, 1.8, 1], opacity: [0.3, 0.8, 0.3] }} transition={{ duration: 3, repeat: Infinity, delay: 1 }} />
                
                <div className="absolute bottom-6 left-6 right-6 grid grid-cols-2 gap-4">
                    <motion.div whileHover={{ y: -5 }} className={`${cardBg} p-4 rounded backdrop-blur-md shadow-lg`}>
                        <div className="text-orange-600 text-2xl font-bold">98.5%</div>
                        <div className={`${cardText} text-xs uppercase font-medium`}>Compliance Score</div>
                        <div className="w-full bg-stone-200 dark:bg-stone-800 h-1 mt-2 rounded overflow-hidden">
                            <motion.div className="bg-orange-500 h-full" initial={{ width: 0 }} animate={{ width: "98.5%" }} transition={{ duration: 1.5, delay: 1.5 }} />
                        </div>
                    </motion.div>
                    <motion.div whileHover={{ y: -5 }} className={`${cardBg} p-4 rounded backdrop-blur-md shadow-lg`}>
                        <div className="text-lime-600 text-2xl font-bold">200+</div>
                        <div className={`${cardText} text-xs uppercase font-medium`}>Projects Audited</div>
                        <div className="flex gap-1 mt-2">
                            <div className="w-2 h-2 bg-lime-500 rounded-full animate-pulse"></div>
                            <div className="w-2 h-2 bg-lime-500/50 rounded-full"></div>
                            <div className="w-2 h-2 bg-lime-500/30 rounded-full"></div>
                        </div>
                    </motion.div>
                </div>
            </div>
        </motion.div>
      </div>

      <div className={`w-full border-t py-3 relative z-20 overflow-hidden transition-colors duration-500 ${tickerBg}`}>
          <div className="max-w-7xl mx-auto px-6 flex items-center justify-between gap-4">
              <div className="flex items-center gap-4 flex-grow overflow-hidden">
                  <div className="flex items-center gap-2 text-orange-600 shrink-0">
                      <Activity size={16} className="animate-pulse" />
                      <span className="text-xs font-bold uppercase tracking-wider">Live Updates</span>
                  </div>
                  <div className="h-4 w-px bg-stone-400 dark:bg-stone-700 shrink-0"></div>
                  <motion.div 
                      className={`flex gap-12 whitespace-nowrap text-xs font-mono ${theme === 'dark' ? 'text-stone-400' : 'text-stone-600'}`}
                      animate={{ x: [0, -500] }}
                      transition={{ duration: 20, repeat: Infinity, ease: "linear" }}
                  >
                      <span>• New Site Audit Started: Sector 62, Noida</span>
                      <span>• Safety Compliance Verified: Project Alpha</span>
                      <span>• MEP Design Approved: Retail Hub Mumbai</span>
                      <span>• Vendor Verification: 100% Complete</span>
                      <span>• New Site Audit Started: Sector 62, Noida</span>
                  </motion.div>
              </div>
              <div className={`hidden md:flex items-center gap-2 border-l pl-4 shrink-0 ${theme === 'dark' ? 'border-stone-700' : 'border-stone-300'}`}>
                  <div className="w-2 h-2 rounded-full bg-orange-500 animate-pulse"></div>
                  <LiveClock theme={theme} />
              </div>
          </div>
      </div>
    </section>
  );
};

const Metrics = ({ theme }) => {
    const stats = [
        { label: "Projects Handled", value: "350+", icon: Layers },
        { label: "Cities Covered", value: "42", icon: MapPin },
        { label: "Risk Mitigated", value: "100%", icon: ShieldCheck },
        { label: "Cost Optimized", value: "~18%", icon: BarChart3 },
    ];

    return (
        <section className="bg-lime-800 py-12 border-y border-lime-700 relative overflow-hidden">
             <div className="absolute top-0 left-0 w-full h-full opacity-10" 
                style={{ backgroundImage: 'radial-gradient(#fff 1px, transparent 1px)', backgroundSize: '20px 20px' }}>
            </div>
            <div className="max-w-7xl mx-auto px-6 grid grid-cols-2 md:grid-cols-4 gap-8 relative z-10">
                {stats.map((stat, index) => (
                    <motion.div 
                        key={index}
                        initial={{ opacity: 0, y: 20 }}
                        whileInView={{ opacity: 1, y: 0 }}
                        viewport={{ once: true }}
                        transition={{ delay: index * 0.1 }}
                        className="text-center"
                    >
                        <div className="flex justify-center mb-3 text-white/90">
                            <stat.icon size={28} strokeWidth={1.5} />
                        </div>
                        <div className="text-3xl md:text-4xl font-bold text-white mb-1">{stat.value}</div>
                        <div className="text-lime-100 text-sm font-medium uppercase tracking-wider">{stat.label}</div>
                    </motion.div>
                ))}
            </div>
        </section>
    );
}

const ServiceCard = ({ icon: Icon, title, desc, img, items, theme }) => (
  <motion.div 
    className={`group relative border transition-colors duration-300 rounded-lg overflow-hidden shadow-sm hover:shadow-xl ${theme === 'dark' ? 'bg-stone-900 border-stone-800 hover:border-orange-500' : 'bg-white border-stone-200 hover:border-orange-500'}`}
    whileHover={{ y: -5 }}
  >
    <div className="h-48 overflow-hidden relative">
        <div className={`absolute inset-0 transition-all z-10 ${theme === 'dark' ? 'bg-stone-950/40 group-hover:bg-transparent' : 'bg-stone-900/10 group-hover:bg-transparent'}`}></div>
        <img src={img} alt={title} className="w-full h-full object-cover transform group-hover:scale-110 transition-transform duration-700" />
    </div>
    <div className="p-8">
      <div className={`w-12 h-12 rounded-lg flex items-center justify-center mb-6 transition-colors ${theme === 'dark' ? 'bg-stone-800 group-hover:bg-orange-600' : 'bg-stone-100 group-hover:bg-orange-600'}`}>
        <Icon size={24} className={`transition-colors ${theme === 'dark' ? 'text-stone-300 group-hover:text-white' : 'text-stone-700 group-hover:text-white'}`} />
      </div>
      <h3 className={`text-xl font-bold mb-3 ${theme === 'dark' ? 'text-white' : 'text-stone-900'}`}>{title}</h3>
      <p className={`text-sm leading-relaxed mb-6 ${theme === 'dark' ? 'text-stone-400' : 'text-stone-600'}`}>{desc}</p>
      <ul className="space-y-2">
          {items.map((item, i) => (
              <li key={i} className={`flex items-start gap-2 text-xs font-medium ${theme === 'dark' ? 'text-stone-500' : 'text-stone-500'}`}>
                  <CheckCircle2 size={14} className="text-lime-600 mt-0.5 shrink-0" />
                  {item}
              </li>
          ))}
      </ul>
    </div>
  </motion.div>
);

const Services = ({ theme }) => {
    const services = [
        {
            title: "Interior & MEP Design",
            desc: "Integrated interior planning synchronized with HVAC, electrical, plumbing & fire systems.",
            icon: Building2,
            img: "https://images.unsplash.com/photo-1497366216548-37526070297c?q=80&w=2069&auto=format&fit=crop",
            items: ["Execution-ready technical drawings", "Space planning", "System integration"]
        },
        {
            title: "Site Feasibility & SDR",
            desc: "Comprehensive location analysis and investment risk evaluation before execution.",
            icon: MapPin,
            img: "https://images.unsplash.com/photo-1486312338219-ce68d2c6f44d?q=80&w=2072&auto=format&fit=crop",
            items: ["Site Due Diligence Reports", "Utility & compliance checks", "Risk forecasting"]
        },
        {
            title: "Auditing Services",
            desc: "Rigorous verification of design, costs, and quality to ensure project integrity.",
            icon: FileCheck,
            img: "https://images.unsplash.com/photo-1554224155-8d04cb21cd6c?q=80&w=2070&auto=format&fit=crop",
            items: ["BOQ & Cost Audits", "Design Compliance", "Quality execution checks"]
        },
        {
            title: "Background Verification (BGV)",
            desc: "Mitigate corporate risk through thorough vendor, contractor, and employee checks.",
            icon: ShieldCheck,
            img: "https://images.unsplash.com/photo-1454165804606-c3d57bc86b40?q=80&w=2070&auto=format&fit=crop",
            items: ["Vendor due diligence", "Contractor history", "Corporate compliance"]
        }
    ];

    return (
        <section id="services" className={`py-24 transition-colors duration-500 ${theme === 'dark' ? 'bg-stone-950' : 'bg-stone-50'}`}>
            <div className="max-w-7xl mx-auto px-6">
                <div className="text-center max-w-3xl mx-auto mb-16">
                    <h2 className={`text-3xl md:text-4xl font-bold mb-4 ${theme === 'dark' ? 'text-white' : 'text-stone-900'}`}>Core Competencies</h2>
                    <div className="h-1 w-20 bg-orange-600 mx-auto mb-6"></div>
                    <p className={`${theme === 'dark' ? 'text-stone-400' : 'text-stone-600'}`}>
                        We combine technical precision with strategic foresight. Our services are designed to support the entire infrastructure lifecycle, from feasibility to final audit.
                    </p>
                </div>
                <div className="grid md:grid-cols-2 lg:grid-cols-4 gap-8">
                    {services.map((s, i) => (
                        <ServiceCard key={i} {...s} theme={theme} />
                    ))}
                </div>
            </div>
        </section>
    );
};

const About = ({ theme }) => {
    const bgClass = theme === 'dark' ? 'bg-stone-900' : 'bg-white';
    const textTitle = theme === 'dark' ? 'text-white' : 'text-stone-900';
    const textBody = theme === 'dark' ? 'text-stone-300' : 'text-stone-600';

    return (
        <section id="about" className={`py-24 overflow-hidden transition-colors duration-500 ${bgClass}`}>
            <div className="max-w-7xl mx-auto px-6">
                <div className="grid lg:grid-cols-2 gap-16 items-center">
                    <motion.div initial={{ opacity: 0, x: -50 }} whileInView={{ opacity: 1, x: 0 }} viewport={{ once: true }}>
                         <h2 className={`text-3xl md:text-4xl font-bold mb-6 leading-tight ${textTitle}`}>
                            We don't just plan. <br/>
                            We <span className="text-orange-600">verify, audit, and execute.</span>
                        </h2>
                        <p className={`text-lg mb-6 leading-relaxed ${textBody}`}>
                            Infrabique Services operates at the intersection of engineering and intelligence. We provide corporate clients with the technical clarity needed to make high-stakes infrastructure decisions.
                        </p>
                        <p className={`mb-8 leading-relaxed ${textBody}`}>
                            Our methodology is rooted in data-backed execution and strict compliance. Whether it is a new retail rollout or a corporate headquarters, we eliminate ambiguity through rigorous site due diligence and technical auditing.
                        </p>
                        <div className="grid grid-cols-2 gap-6">
                            <div className={`flex flex-col gap-2 border-l-2 border-orange-200 pl-4`}>
                                <span className={`font-bold ${textTitle}`}>Data-Backed</span>
                                <span className="text-stone-500 text-sm">Every decision supported by metrics.</span>
                            </div>
                            <div className="flex flex-col gap-2 border-l-2 border-lime-200 pl-4">
                                <span className={`font-bold ${textTitle}`}>Risk Mitigation</span>
                                <span className="text-stone-500 text-sm">Proactive identification of pitfalls.</span>
                            </div>
                        </div>
                        <motion.button whileTap={{ scale: 0.95 }} onClick={() => scrollToSection('contact')} className="mt-10 text-orange-600 font-semibold flex items-center gap-2 hover:gap-4 transition-all">
                            Read our Corporate Profile <ArrowRight size={18} />
                        </motion.button>
                    </motion.div>
                    <motion.div initial={{ opacity: 0, x: 50 }} whileInView={{ opacity: 1, x: 0 }} viewport={{ once: true }} className="relative">
                        <div className="absolute top-0 right-0 w-3/4 h-3/4 bg-lime-100 rounded-lg -z-10 translate-x-8 -translate-y-8"></div>
                         <img src="https://images.unsplash.com/photo-1504917595217-d4dc5ebe6122?q=80&w=2070&auto=format&fit=crop" alt="Site Audit" className="rounded-lg shadow-2xl w-full h-auto object-cover" />
                        <div className={`absolute bottom-0 left-0 p-6 rounded-tr-xl shadow-lg max-w-xs ${theme === 'dark' ? 'bg-stone-800 text-white' : 'bg-stone-900 text-white'}`}>
                            <div className="flex items-center gap-3 mb-2">
                                <ShieldCheck className="text-orange-500" />
                                <span className="font-bold text-lg">Compliance First</span>
                            </div>
                            <p className="text-stone-400 text-xs">Adhering to global standards in safety, engineering, and vendor verification.</p>
                        </div>
                    </motion.div>
                </div>
            </div>
        </section>
    );
};

const Clients = ({ theme }) => {
    const clients = [
      { 
        name: "Burger Singh", 
        logo: "https://upload.wikimedia.org/wikipedia/en/thumb/e/e3/Burger_Singh_Logo.png/220px-Burger_Singh_Logo.png",
        fallbackText: "BURGER SINGH",
        color: "text-orange-600"
      },
      { 
        name: "Domino's", 
        logo: "https://upload.wikimedia.org/wikipedia/commons/thumb/3/3e/Domino%27s_pizza_logo.svg/800px-Domino%27s_pizza_logo.svg.png",
        fallbackText: "Domino's",
        color: "text-blue-600"
      },
      { 
        name: "Hong's Kitchen", 
        logo: "https://b.zmtcdn.com/data/brand_creatives/logos/1a9db19318a4a274df280227977430d4_1611384074.png", 
        fallbackText: "Hong's Kitchen",
        color: "text-red-600"
      },
      { 
        name: "Popeyes", 
        logo: "https://upload.wikimedia.org/wikipedia/commons/thumb/8/80/Popeyes_Louisiana_Kitchen_logo.svg/1200px-Popeyes_Louisiana_Kitchen_logo.svg.png",
        fallbackText: "Popeyes",
        color: "text-orange-500"
      },
      { 
        name: "Aditya Birla Group", 
        logo: "https://upload.wikimedia.org/wikipedia/en/thumb/e/e6/Aditya_Birla_Group_Logo.svg/1200px-Aditya_Birla_Group_Logo.svg.png",
        fallbackText: "ADITYA BIRLA",
        color: "text-red-700"
      },
    ];
  
    return (
      <section className={`py-16 border-b transition-colors duration-500 ${theme === 'dark' ? 'bg-stone-950 border-stone-800' : 'bg-white border-stone-100'}`}>
        <div className="max-w-7xl mx-auto px-6 text-center">
          <p className={`font-medium mb-12 uppercase tracking-widest text-sm ${theme === 'dark' ? 'text-stone-500' : 'text-stone-500'}`}>
            Trusted as PMC Engineers For Leading Brands
          </p>
          <div className="flex flex-wrap justify-center gap-12 md:gap-20 items-center">
            {clients.map((client, index) => (
              <div key={index} className={`relative group h-12 flex items-center justify-center`}>
                <img 
                  src={client.logo} 
                  alt={client.name} 
                  className={`h-full w-auto object-contain filter grayscale opacity-70 group-hover:grayscale-0 group-hover:opacity-100 transition-all duration-300`}
                  onError={(e) => {
                    e.target.style.display = 'none';
                    e.target.nextSibling.style.display = 'block';
                  }}
                />
                <span 
                    className={`hidden text-xl font-black uppercase tracking-tight ${client.color} ${theme === 'dark' && client.color.includes('text-stone') ? 'text-white' : ''}`}
                    style={{ display: 'none' }}
                >
                    {client.fallbackText}
                </span>
              </div>
            ))}
          </div>
        </div>
      </section>
    );
  };

const Industries = ({ theme }) => {
    const industries = [
        "Retail & QSR Chains",
        "Corporate Offices",
        "Commercial Infrastructure",
        "Industrial & Warehousing"
    ];

    return (
        <section id="industries" className="py-20 bg-stone-900 text-white border-t border-stone-800">
             <div className="max-w-7xl mx-auto px-6">
                <div className="flex flex-col md:flex-row justify-between items-end mb-12 gap-6">
                    <div>
                        <h2 className="text-3xl font-bold mb-2">Industries Served</h2>
                        <p className="text-stone-400">Tailored technical solutions for diverse sectors.</p>
                    </div>
                    <div className="flex gap-2">
                        <div className="h-px w-24 bg-orange-600 self-center"></div>
                    </div>
                </div>
                <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
                    {industries.map((ind, i) => (
                        <div key={i} className="bg-stone-800/50 p-6 border border-stone-700 hover:border-orange-500/50 transition-colors group">
                            <div className="flex justify-between items-start mb-8">
                                <Grid className="text-orange-500/50 group-hover:text-orange-400" />
                                <ArrowRight size={16} className="text-stone-600 group-hover:text-orange-400 -rotate-45 group-hover:rotate-0 transition-transform" />
                            </div>
                            <h3 className="font-semibold text-lg text-stone-200 group-hover:text-white">{ind}</h3>
                        </div>
                    ))}
                </div>
             </div>
        </section>
    );
};

const Careers = ({ theme, onOpenModal }) => {
    const opportunities = [
        { role: "Senior Civil Engineer", loc: "Mumbai", type: "Full-time", exp: "5-8 Years" },
        { role: "MEP Design Lead", loc: "Pune", type: "Full-time", exp: "6+ Years" },
        { role: "Site Audit Specialist", loc: "Pan-India", type: "Contract", exp: "3-5 Years" }
    ];

    const bgClass = theme === 'dark' ? 'bg-stone-950 border-stone-800' : 'bg-white border-stone-100';
    const textTitle = theme === 'dark' ? 'text-white' : 'text-stone-900';
    const cardBg = theme === 'dark' ? 'bg-stone-900 border-stone-800 hover:bg-stone-800' : 'bg-stone-50 border-stone-200 hover:bg-white';

    return (
        <section id="careers" className={`py-24 relative overflow-hidden border-b transition-colors duration-500 ${bgClass}`}>
             <div className="absolute top-0 right-0 w-64 h-64 bg-orange-500/10 rounded-full blur-3xl -z-10 translate-x-1/2 -translate-y-1/2"></div>
             <div className="max-w-7xl mx-auto px-6">
                <div className="flex flex-col md:flex-row justify-between items-end mb-16 gap-6">
                    <div>
                        <div className="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-orange-100 text-orange-800 text-xs font-bold uppercase tracking-wider mb-4">
                            We are Hiring
                        </div>
                        <h2 className={`text-3xl md:text-4xl font-bold ${textTitle}`}>Build the Infrastructure of Tomorrow</h2>
                    </div>
                    <p className="text-stone-500 max-w-md text-right md:text-left">
                        Join a team of technical experts dedicated to precision, safety, and engineering excellence.
                    </p>
                </div>
                <div className="grid md:grid-cols-3 gap-6">
                    {opportunities.map((job, i) => (
                        <div key={i} className={`group border p-6 rounded-xl hover:border-orange-500 hover:shadow-lg transition-all duration-300 ${cardBg}`}>
                            <div className="flex justify-between items-start mb-4">
                                <div className={`p-2 rounded shadow-sm transition-colors ${theme === 'dark' ? 'bg-stone-800 group-hover:bg-orange-600 text-white' : 'bg-white group-hover:bg-orange-600 group-hover:text-white'}`}>
                                    <Briefcase size={20} />
                                </div>
                                <span className={`text-xs font-semibold px-2 py-1 rounded ${theme === 'dark' ? 'bg-stone-800 text-stone-300' : 'bg-stone-200 text-stone-600'}`}>{job.type}</span>
                            </div>
                            <h3 className={`text-lg font-bold mb-2 ${textTitle}`}>{job.role}</h3>
                            <div className="flex flex-col gap-2 mb-6 text-sm text-stone-500">
                                <div className="flex items-center gap-2">
                                    <MapPin size={14} /> {job.loc}
                                </div>
                                <div className="flex items-center gap-2">
                                    <Activity size={14} /> {job.exp}
                                </div>
                            </div>
                            <motion.button whileTap={{ scale: 0.95 }} onClick={() => onOpenModal(job.role)} className={`w-full py-2 border rounded font-medium transition-all ${theme === 'dark' ? 'border-stone-700 text-stone-300 hover:bg-stone-700 hover:text-white' : 'border-stone-300 text-stone-600 hover:bg-stone-900 hover:text-white hover:border-stone-900'}`}>
                                Apply Now
                            </motion.button>
                        </div>
                    ))}
                </div>
                <div className="mt-12 bg-stone-900 rounded-2xl p-8 md:p-12 relative overflow-hidden flex flex-col md:flex-row items-center justify-between gap-8">
                     <div className="absolute inset-0 opacity-20" style={{ backgroundImage: 'radial-gradient(#ea580c 1px, transparent 1px)', backgroundSize: '20px 20px' }}></div>
                    <div className="relative z-10">
                        <h3 className="text-2xl font-bold text-white mb-2">Don't see the right fit?</h3>
                        <p className="text-stone-400">We are always looking for exceptional talent. Send us your open application.</p>
                    </div>
                    <div className="relative z-10 flex gap-4">
                        <motion.a whileTap={{ scale: 0.95 }} href="mailto:careers@infrabique.com" className="bg-orange-600 hover:bg-orange-700 text-white px-6 py-3 rounded font-semibold transition-colors">
                            Email CV
                        </motion.a>
                    </div>
                </div>
             </div>
        </section>
    );
};

const Contact = ({ theme }) => {
    const [formData, setFormData] = useState({
        firstName: '',
        lastName: '',
        companyName: '',
        service: 'Interior & MEP Design',
        details: ''
    });

    const handleChange = (e) => {
        setFormData({ ...formData, [e.target.name]: e.target.value });
    };

    const handleSendRequest = (e) => {
        e.preventDefault();
        const subject = `Project Inquiry from ${formData.firstName} ${formData.lastName} - ${formData.companyName}`;
        const body = `Name: ${formData.firstName} ${formData.lastName}\nCompany: ${formData.companyName}\nService Interest: ${formData.service}\n\nProject Details:\n${formData.details}`;
        window.location.href = `mailto:info@infrabique.com?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(body)}`;
    };

    const bgClass = theme === 'dark' ? 'bg-stone-900' : 'bg-stone-50';
    const cardBg = theme === 'dark' ? 'bg-stone-800' : 'bg-white';
    const inputClass = theme === 'dark' ? 'text-white border-stone-600' : 'text-stone-900 border-stone-300';

    return (
        <section id="contact" className={`py-24 relative transition-colors duration-500 ${bgClass}`}>
             <div className="max-w-5xl mx-auto px-6 relative z-10">
                <div className={`rounded-2xl shadow-xl overflow-hidden grid md:grid-cols-5 ${cardBg}`}>
                    <div className="bg-stone-900 text-white p-10 md:col-span-2 flex flex-col justify-between">
                        <div>
                            <h3 className="text-2xl font-bold mb-6">Let's Discuss</h3>
                            <p className="text-stone-400 mb-8 text-sm">Start your project with technical confidence. Reach out for a consultation or audit request.</p>
                            <div className="space-y-6">
                                <div className="flex items-start gap-4">
                                    <Mail className="text-orange-500 mt-1" size={20} />
                                    <div><p className="text-xs text-stone-500 uppercase font-bold">Email</p><p className="text-sm">info@infrabique.com</p></div>
                                </div>
                                <div className="flex items-start gap-4">
                                    <MapPin className="text-orange-500 mt-1" size={20} />
                                    <div><p className="text-xs text-stone-500 uppercase font-bold">HQ</p><p className="text-sm">Jodhpur, Rajasthan</p></div>
                                </div>
                            </div>
                        </div>
                        <div className="mt-12">
                             <div className="flex gap-3 text-stone-500">
                                <motion.a whileTap={{ scale: 0.9 }} href="https://www.linkedin.com/company/infrabiqueservices/" target="_blank" rel="noopener noreferrer" className="w-9 h-9 rounded-full bg-stone-800 flex items-center justify-center hover:bg-orange-600 hover:text-white transition-colors"><Linkedin size={18} /></motion.a>
                                <motion.a whileTap={{ scale: 0.9 }} href="#" className="w-9 h-9 rounded-full bg-stone-800 flex items-center justify-center hover:bg-orange-600 hover:text-white transition-colors"><Twitter size={18} /></motion.a>
                                <motion.a whileTap={{ scale: 0.9 }} href="https://www.instagram.com/infrabique?igsh=OG5nbGppNG9hbjdo" target="_blank" rel="noopener noreferrer" className="w-9 h-9 rounded-full bg-stone-800 flex items-center justify-center hover:bg-orange-600 hover:text-white transition-colors"><Instagram size={18} /></motion.a>
                                <motion.a whileTap={{ scale: 0.9 }} href="#" className="w-9 h-9 rounded-full bg-stone-800 flex items-center justify-center hover:bg-orange-600 hover:text-white transition-colors"><Facebook size={18} /></motion.a>
                                <motion.a whileTap={{ scale: 0.9 }} href="https://youtube.com/@infrabiquequeryroom?si=HWAWcXkB6Rpf2mbe" target="_blank" rel="noopener noreferrer" className="w-9 h-9 rounded-full bg-stone-800 flex items-center justify-center hover:bg-orange-600 hover:text-white transition-colors"><Youtube size={18} /></motion.a>
                             </div>
                        </div>
                    </div>
                    <div className="p-10 md:col-span-3">
                        <form className="space-y-6" onSubmit={handleSendRequest}>
                            <div className="grid md:grid-cols-2 gap-6">
                                <div><label className="block text-xs font-bold text-stone-500 uppercase mb-2">First Name</label><input type="text" name="firstName" value={formData.firstName} onChange={handleChange} className={`w-full border-b py-2 focus:border-orange-500 outline-none transition-colors bg-transparent ${inputClass}`} placeholder="John" required /></div>
                                <div><label className="block text-xs font-bold text-stone-500 uppercase mb-2">Last Name</label><input type="text" name="lastName" value={formData.lastName} onChange={handleChange} className={`w-full border-b py-2 focus:border-orange-500 outline-none transition-colors bg-transparent ${inputClass}`} placeholder="Doe" required /></div>
                            </div>
                            <div><label className="block text-xs font-bold text-stone-500 uppercase mb-2">Company Name</label><input type="text" name="companyName" value={formData.companyName} onChange={handleChange} className={`w-full border-b py-2 focus:border-orange-500 outline-none transition-colors bg-transparent ${inputClass}`} placeholder="Enterprise Ltd." required /></div>
                            <div><label className="block text-xs font-bold text-stone-500 uppercase mb-2">Service Interest</label><select name="service" value={formData.service} onChange={handleChange} className={`w-full border-b py-2 focus:border-orange-500 outline-none transition-colors bg-transparent ${inputClass}`}><option className="text-stone-900">Interior & MEP Design</option><option className="text-stone-900">Site Feasibility / SDR</option><option className="text-stone-900">Auditing Services</option><option className="text-stone-900">Background Verification</option><option className="text-stone-900">Other</option></select></div>
                            <div><label className="block text-xs font-bold text-stone-500 uppercase mb-2">Project Details</label><textarea name="details" value={formData.details} onChange={handleChange} className={`w-full border-b py-2 focus:border-orange-500 outline-none transition-colors h-24 resize-none bg-transparent ${inputClass}`} placeholder="Tell us about your requirements..." required></textarea></div>
                            <motion.button whileTap={{ scale: 0.95 }} type="submit" className="bg-stone-900 text-white px-8 py-3 rounded font-semibold hover:bg-orange-600 transition-colors w-full md:w-auto">Send Request</motion.button>
                        </form>
                    </div>
                </div>
             </div>
        </section>
    );
};

const Footer = ({ theme }) => {
    return (
        <footer className={`py-12 border-t text-sm transition-colors duration-500 ${theme === 'dark' ? 'bg-stone-950 text-stone-400 border-stone-800' : 'bg-stone-100 text-stone-500 border-stone-200'}`}>
            <div className="max-w-7xl mx-auto px-6 flex flex-col md:flex-row justify-between items-center gap-6">
                <div className="flex flex-col gap-4">
                    <div className="flex items-center gap-2">
                        <img src={LOGO_URL} alt="Infrabique Logo" className="w-8 h-8 object-contain bg-white rounded-sm" />
                        <span className={`font-bold tracking-tight ${theme === 'dark' ? 'text-stone-200' : 'text-stone-800'}`}>INFRABIQUE SERVICES</span>
                    </div>
                    <div className="flex gap-4">
                        <motion.a whileTap={{ scale: 0.9 }} href="https://www.linkedin.com/company/infrabiqueservices/" target="_blank" rel="noopener noreferrer" className="hover:text-orange-500 transition-colors"><Linkedin size={18} /></motion.a>
                        <motion.a whileTap={{ scale: 0.9 }} href="#" className="hover:text-orange-500 transition-colors"><Twitter size={18} /></motion.a>
                        <motion.a whileTap={{ scale: 0.9 }} href="https://www.instagram.com/infrabique?igsh=OG5nbGppNG9hbjdo" target="_blank" rel="noopener noreferrer" className="hover:text-orange-500 transition-colors"><Instagram size={18} /></motion.a>
                        <motion.a whileTap={{ scale: 0.9 }} href="#" className="hover:text-orange-500 transition-colors"><Facebook size={18} /></motion.a>
                        <motion.a whileTap={{ scale: 0.9 }} href="https://youtube.com/@infrabiquequeryroom?si=HWAWcXkB6Rpf2mbe" target="_blank" rel="noopener noreferrer" className="hover:text-orange-500 transition-colors"><Youtube size={18} /></motion.a>
                    </div>
                </div>
                <div className="flex gap-8">
                    <a href="#" className="hover:text-orange-500 transition-colors">Privacy Policy</a>
                    <a href="#" className="hover:text-orange-500 transition-colors">Terms of Service</a>
                    <a href="#" className="hover:text-orange-500 transition-colors">Sitemap</a>
                </div>
                <div className={theme === 'dark' ? 'text-stone-500' : 'text-stone-400'}>
                    &copy; 2026 Infrabique. All rights reserved.
                </div>
            </div>
        </footer>
    );
};

// --- Main App Component ---

const App = () => {
  const [loading, setLoading] = useState(true);
  const [scrolled, setScrolled] = useState(false);
  const [theme, setTheme] = useState('light');
  const [modalJob, setModalJob] = useState(null);

  const toggleTheme = () => {
    setTheme(prevTheme => prevTheme === 'light' ? 'dark' : 'light');
  };

  useEffect(() => {
    const handleScroll = () => {
      setScrolled(window.scrollY > 50);
    };
    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
  }, []);

  return (
    <div className={`font-sans selection:bg-orange-200 selection:text-orange-900 transition-colors duration-500 ${theme === 'dark' ? 'bg-stone-950 text-white' : 'bg-white text-stone-900'}`}>
        <AnimatePresence>
            {loading && <IntroOverlay onComplete={() => setLoading(false)} />}
        </AnimatePresence>

        <ApplicationModal 
          isOpen={!!modalJob} 
          onClose={() => setModalJob(null)} 
          jobRole={modalJob} 
          theme={theme} 
        />

        {!loading && (
            <motion.div
                initial={{ opacity: 0 }}
                animate={{ opacity: 1 }}
                transition={{ duration: 1 }}
            >
                <Navbar scrolled={scrolled} theme={theme} toggleTheme={toggleTheme} />
                <Hero theme={theme} />
                <Metrics theme={theme} />
                <About theme={theme} />
                <Clients theme={theme} />
                <Services theme={theme} />
                <Industries theme={theme} />
                <Careers theme={theme} onOpenModal={setModalJob} />
                <Contact theme={theme} />
                <Footer theme={theme} />
            </motion.div>
        )}
    </div>
  );
};

export default App;
