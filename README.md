import { useState, useEffect, useRef } from "react";
import { motion } from "framer-motion";
import { Button } from "@/components/ui/button";
import Typed from "react-typed";
import Confetti from "react-confetti";
import { FaHeart, FaMusic, FaVolumeMute, FaGift } from "react-icons/fa";

// Replace these with actual image URLs
const backgrounds = [
  "https://images.unsplash.com/photo-1513151233558-d860c5398176?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80",
  "https://images.unsplash.com/photo-1530103862676-de8c9debad1d?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80",
  "https://images.unsplash.com/photo-1464349095431-e9a21285b5f3?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1936&q=80"
];

// Birthday music
const birthdaySong = "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3";

export default function BirthdayPage() {
  const [showLetter, setShowLetter] = useState(false);
  const [currentBg, setCurrentBg] = useState(0);
  const [showConfetti, setShowConfetti] = useState(true);
  const [isMusicPlaying, setIsMusicPlaying] = useState(false);
  const [showGallery, setShowGallery] = useState(false);
  const audioRef = useRef(null);
  const [windowSize, setWindowSize] = useState({
    width: typeof window !== 'undefined' ? window.innerWidth : 0,
    height: typeof window !== 'undefined' ? window.innerHeight : 0,
  });

  // Handle window resize
  useEffect(() => {
    const handleResize = () => {
      setWindowSize({
        width: window.innerWidth,
        height: window.innerHeight,
      });
    };

    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  // Background rotation
  useEffect(() => {
    if (!backgrounds || backgrounds.length === 0) return;
    const interval = setInterval(() => {
      setCurrentBg((prev) => (prev + 1) % backgrounds.length);
    }, 5000);
    return () => clearInterval(interval);
  }, []);

  // Confetti timeout
  useEffect(() => {
    const timer = setTimeout(() => {
      setShowConfetti(false);
    }, 10000);
    return () => clearTimeout(timer);
  }, []);

  // Music control
  const toggleMusic = () => {
    if (isMusicPlaying) {
      audioRef.current.pause();
    } else {
      audioRef.current.play();
    }
    setIsMusicPlaying(!isMusicPlaying);
  };

  return (
    <div className="relative">
      {/* Background music (hidden) */}
      <audio ref={audioRef} loop src={birthdaySong} />

      {/* Confetti */}
      {showConfetti && (
        <Confetti
          width={windowSize.width}
          height={windowSize.height}
          recycle={false}
          numberOfPieces={500}
        />
      )}

      {/* Main content */}
      <motion.div 
        className="min-h-screen flex flex-col items-center justify-center bg-cover bg-center text-center relative overflow-hidden"
        style={{ 
          backgroundImage: `url(${backgrounds[currentBg]})`,
          backgroundColor: "#111",
          backgroundSize: "cover",
          backgroundPosition: "center",
          transition: "background-image 1s ease-in-out"
        }}
        initial={{ opacity: 0 }}
        animate={{ opacity: 1 }}
        transition={{ duration: 1 }}
      >
        {/* Overlay */}
        <motion.div 
          className="absolute inset-0 bg-black bg-opacity-60 backdrop-blur-sm" 
          initial={{ opacity: 0 }} 
          animate={{ opacity: 0.7 }}
          transition={{ duration: 1 }}
        ></motion.div>
        
        {/* Header */}
        <motion.h1 
          className="text-4xl sm:text-5xl md:text-6xl lg:text-7xl font-extrabold text-white drop-shadow-2xl relative z-10 px-4"
          initial={{ opacity: 0, y: -50 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 1, delay: 0.3 }}
        >
          🎂 Happy Birthday, Shruti Pandey! 🎉
        </motion.h1>
        
        {/* Age reveal */}
        <motion.div
          className="mt-4 text-2xl md:text-3xl text-pink-300 font-bold z-10"
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          transition={{ duration: 1, delay: 0.5 }}
        >
          Welcome to the fabulous 18! ✨
        </motion.div>
        
        {/* Typed animation */}
        <motion.div 
          className="mt-6 p-4 sm:p-6 bg-black bg-opacity-60 text-green-300 font-mono rounded-lg shadow-2xl backdrop-blur-sm relative border border-green-400 max-w-md mx-4"
          initial={{ scale: 0.8, opacity: 0 }}
          animate={{ scale: 1, opacity: 1 }}
          transition={{ duration: 0.8, delay: 0.6 }}
        >
          <Typed
            strings={[
              "echo 'Happy Birthday, Shruti Pandey!'", 
              "print('Wishing you an amazing year ahead!')", 
              "System.out.println('Have an awesome birthday!');",
              "console.log('May all your dreams come true!');"
            ]}
            typeSpeed={50}
            backSpeed={30}
            loop
          />
        </motion.div>
        
        {/* Action buttons */}
        <motion.div 
          className="flex flex-col sm:flex-row gap-4 mt-8 z-10"
          initial={{ scale: 0.8, opacity: 0 }}
          animate={{ scale: 1, opacity: 1 }}
          transition={{ duration: 0.8, delay: 1 }}
        >
          <Button 
            className="bg-pink-500 hover:bg-pink-600 text-white px-6 py-3 rounded-xl shadow-lg text-lg transition-all hover:scale-105 backdrop-blur-sm border-2 border-white flex items-center gap-2"
            onClick={() => setShowLetter(true)}
          >
            <FaHeart /> Open Letter
          </Button>
          
          <Button 
            className="bg-purple-500 hover:bg-purple-600 text-white px-6 py-3 rounded-xl shadow-lg text-lg transition-all hover:scale-105 backdrop-blur-sm border-2 border-white flex items-center gap-2"
            onClick={() => setShowGallery(true)}
          >
            <FaGift /> Memory Gallery
          </Button>
          
          <Button 
            className="bg-blue-500 hover:bg-blue-600 text-white px-6 py-3 rounded-xl shadow-lg text-lg transition-all hover:scale-105 backdrop-blur-sm border-2 border-white flex items-center gap-2"
            onClick={toggleMusic}
          >
            {isMusicPlaying ? <FaVolumeMute /> : <FaMusic />} 
            {isMusicPlaying ? "Mute" : "Play Music"}
          </Button>
        </motion.div>
        
        {/* Floating hearts */}
        {[...Array(10)].map((_, i) => (
          <motion.div
            key={i}
            className="absolute text-pink-400 text-2xl"
            initial={{
              x: Math.random() * windowSize.width,
              y: -50,
              opacity: 0
            }}
            animate={{
              y: windowSize.height + 50,
              opacity: [0, 1, 0],
              rotate: Math.random() * 360
            }}
            transition={{
              duration: 5 + Math.random() * 10,
              repeat: Infinity,
              delay: Math.random() * 5
            }}
          >
            <FaHeart />
          </motion.div>
        ))}
      </motion.div>
      
      {/* Birthday letter modal */}
      {showLetter && (
        <motion.div 
          className="fixed inset-0 flex items-center justify-center bg-black bg-opacity-90 p-4 z-50"
          initial={{ opacity: 0, scale: 0.8 }}
          animate={{ opacity: 1, scale: 1 }}
          transition={{ duration: 0.6 }}
        >
          <motion.div 
            className="bg-gray-900 p-6 md:p-8 rounded-2xl shadow-2xl max-w-lg mx-4 text-center border-2 border-pink-500 backdrop-blur-sm relative overflow-y-auto max-h-[90vh]"
            initial={{ scale: 0.8, opacity: 0 }}
            animate={{ scale: 1, opacity: 1 }}
            transition={{ duration: 0.5 }}
          >
            <h2 className="text-2xl md:text-3xl font-bold text-pink-400 mb-4">Dear Shruti,</h2>
            
            <div className="text-left text-base md:text-lg leading-relaxed space-y-4">
              <p>Happy Birthdayyyyyyyyyyy Shruti Qt Pandey 🎉</p>
              
              <p>I hope you liked the small effort I made to wish you on your special 18th birthday.</p>
              
              <p>Wish you the fulfillment of all your fondest dreams. May you be blessed with a year filled with abundant joy, and may this year bring you endless happiness and success. (My Taiji wrote this to me on my birthday, and I think it's good to write it here too since, as you know, I'm not so good in English.)</p>
              
              <p>So yes, that's all—not able to gather much content due to the time constraints of exams.</p>
              
              <p>But yes, once again, Happy Birthday! 🎂🥳</p>
              
              <p className="text-right mt-6 text-pink-300">With love,<br />[Your Name]</p>
            </div>
            
            <motion.div 
              initial={{ scale: 0.8, opacity: 0 }}
              animate={{ scale: 1, opacity: 1 }}
              transition={{ duration: 0.5, delay: 0.3 }}
              className="mt-6"
            >
              <Button 
                className="bg-red-500 hover:bg-red-600 text-white px-6 py-3 rounded-xl shadow-lg transition-all hover:scale-105"
                onClick={() => setShowLetter(false)}
              >
                Close
              </Button>
            </motion.div>
          </motion.div>
        </motion.div>
      )}
      
      {/* Photo gallery modal */}
      {showGallery && (
        <motion.div 
          className="fixed inset-0 flex items-center justify-center bg-black bg-opacity-90 p-4 z-50"
          initial={{ opacity: 0, scale: 0.8 }}
          animate={{ opacity: 1, scale: 1 }}
          transition={{ duration: 0.6 }}
        >
          <motion.div 
            className="bg-gray-900 p-4 md:p-6 rounded-2xl shadow-2xl max-w-4xl w-full mx-4 text-center border-2 border-blue-500 backdrop-blur-sm relative max-h-[90vh] overflow-y-auto"
            initial={{ scale: 0.8, opacity: 0 }}
            animate={{ scale: 1, opacity: 1 }}
            transition={{ duration: 0.5 }}
          >
            <h2 className="text-2xl md:text-3xl font-bold text-blue-400 mb-6">Memory Gallery</h2>
            
            <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
              {backgrounds.map((bg, index) => (
                <motion.div
                  key={index}
                  className="relative group overflow-hidden rounded-lg"
                  whileHover={{ scale: 1.03 }}
                >
                  <img 
                    src={bg} 
                    alt={`Memory ${index + 1}`}
                    className="w-full h-48 object-cover transition-transform group-hover:scale-110"
                  />
                  <div className="absolute inset-0 bg-black bg-opacity-30 flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity">
                    <span className="text-white font-bold">Memory #{index + 1}</span>
                  </div>
                </motion.div>
              ))}
              {/* Add more images here */}
            </div>
            
            <motion.div 
              initial={{ scale: 0.8, opacity: 0 }}
              animate={{ scale: 1, opacity: 1 }}
              transition={{ duration: 0.5, delay: 0.3 }}
              className="mt-6"
            >
              <Button 
                className="bg-red-500 hover:bg-red-600 text-white px-6 py-3 rounded-xl shadow-lg transition-all hover:scale-105"
                onClick={() => setShowGallery(false)}
              >
                Close Gallery
              </Button>
            </motion.div>
          </motion.div>
        </motion.div>
      )}
    </div>
  );
}
