<script setup>
import { onMounted, onUnmounted, ref } from 'vue';

const canvasRef = ref(null);
let animationId;
const petals = [];
const petalCount = 40;

const random = (min, max) => Math.random() * (max - min) + min;

class Petal {
  constructor(canvas) {
    this.canvas = canvas;
    this.x = random(0, canvas.width);
    this.y = random(-canvas.height, 0);
    this.size = random(5, 12);
    this.speed = random(0.5, 1.5);
    this.oscillation = random(1.5, 2.5); // reduced oscillation
    this.angle = random(0, Math.PI * 2);
    this.spin = random(-0.02, 0.02);
    this.opacity = random(0.6, 0.9);
    
    // Slight pink color variations
    const colors = ['#ffd7e6', '#ffc0cb', '#ffb7c5'];
    this.color = colors[Math.floor(random(0, colors.length))];
  }

  update() {
    this.y += this.speed;
    this.x += Math.sin(this.angle) * 0.5; // smoother x movement
    this.angle += this.spin;

    if (this.y > this.canvas.height) {
      this.y = -20;
      this.x = random(0, this.canvas.width);
    }
  }

  draw(ctx) {
    ctx.save();
    ctx.translate(this.x, this.y);
    ctx.rotate(this.angle);
    ctx.globalAlpha = this.opacity;
    ctx.fillStyle = this.color;
    
    // Draw a simple petal shape
    ctx.beginPath();
    ctx.ellipse(0, 0, this.size, this.size / 2, 0, 0, Math.PI * 2);
    ctx.fill();
    
    ctx.restore();
  }
}

const resizeCanvas = () => {
  if (canvasRef.value) {
    canvasRef.value.width = window.innerWidth;
    canvasRef.value.height = window.innerHeight;
  }
};

const animate = () => {
  const canvas = canvasRef.value;
  if (!canvas) return;
  
  const ctx = canvas.getContext('2d');
  ctx.clearRect(0, 0, canvas.width, canvas.height); // Clear canvas

  petals.forEach(petal => {
    // Update petal position
    petal.x += Math.sin(petal.angle) * 0.5; // Sway
    petal.y += petal.speed;
    petal.angle += petal.spin;

    // Reset if out of view
    if (petal.y > canvas.height) {
      petal.y = -20;
      petal.x = Math.random() * canvas.width;
    }

    // Draw petal
    ctx.save();
    ctx.translate(petal.x, petal.y);
    ctx.rotate(petal.angle);
    ctx.globalAlpha = petal.opacity;
    ctx.fillStyle = petal.color;
    
    // Draw simple oval petal
    ctx.beginPath();
    ctx.ellipse(0, 0, petal.size, petal.size / 2, 0, 0, Math.PI * 2);
    ctx.fill();
    ctx.restore();
  });

  animationId = requestAnimationFrame(animate);
};

onMounted(() => {
  resizeCanvas();
  window.addEventListener('resize', resizeCanvas);

  for (let i = 0; i < petalCount; i++) {
    petals.push(new Petal(canvasRef.value));
  }

  animate();
});

onUnmounted(() => {
  window.removeEventListener('resize', resizeCanvas);
  cancelAnimationFrame(animationId);
});
</script>

<template>
  <canvas ref="canvasRef" class="blossom-canvas"></canvas>
</template>

<style scoped>
.blossom-canvas {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none; /* Let clicks pass through */
  z-index: 9999;
}
</style>
