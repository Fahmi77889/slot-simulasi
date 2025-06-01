# slot-simulasi
Simulasi permainan slot berbasis React

import React, { useState } from "react";

const symbols = ["🍒", "🍋", "🔔", "⭐", "7️⃣"];
const spinSound = new Audio("/sounds/spin.mp3");
const winSound = new Audio("/sounds/win.mp3");

export default function SlotGame() {
  const [reels, setReels] = useState(["🍒", "🍋", "🔔"]);
  const [score, setScore] = useState(0);
  const [spinning, setSpinning] = useState(false);
  const [won, setWon] = useState(false);

  const spin = () => {
    if (spinning) return;

    spinSound.play();
    setSpinning(true);
    setWon(false);

    let spinInterval = setInterval(() => {
      const randomReels = [
        symbols[Math.floor(Math.random() * symbols.length)],
        symbols[Math.floor(Math.random() * symbols.length)],
        symbols[Math.floor(Math.random() * symbols.length)],
      ];
      setReels(randomReels);
    }, 100);

    setTimeout(() => {
      clearInterval(spinInterval);
      const finalReels = [
        symbols[Math.floor(Math.random() * symbols.length)],
        symbols[Math.floor(Math.random() * symbols.length)],
        symbols[Math.floor(Math.random() * symbols.length)],
      ];
      setReels(finalReels);

      if (finalReels[0] === finalReels[1] && finalReels[1] === finalReels[2]) {
        setScore(score + 100);
        setWon(true);
        winSound.play();
      } else {
        setScore(score - 10);
      }

      setSpinning(false);
    }, 1500);
  };

  return (
    <div className="min-h-screen flex flex-col items-center justify-center bg-gradient-to-br from-purple-600 to-pink-500 text-white transition-all duration-500">
      <h1 className="text-4xl font-bold mb-6">🎰 Slot Game Simulasi</h1>

      <div className={`flex gap-4 text-6xl mb-4 ${won ? "animate-pulse" : ""}`}>
        {reels.map((symbol, idx) => (
          <div
            key={idx}
            className="w-20 h-20 bg-white rounded-2xl flex items-center justify-center text-black shadow-xl text-5xl"
          >
            {symbol}
          </div>
        ))}
      </div>

      <button
        onClick={spin}
        disabled={spinning}
        className={`px-8 py-3 rounded-full text-xl font-bold transition-all duration-300 shadow-lg ${
          spinning
            ? "bg-gray-300 cursor-not-allowed"
            : "bg-yellow-400 hover:bg-yellow-300 text-black"
        }`}
      >
        {spinning ? "SPINNING..." : "SPIN 🎲"}
      </button>

      <p className="text-xl mt-4">
        Skor: <span className="font-bold">{score}</span>
      </p>
      {won && (
        <p className="mt-2 text-green-300 text-lg animate-bounce">🎉 Jackpot! 🎉</p>
      )}
    </div>
  );
}
