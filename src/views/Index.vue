<template>
  <div class="island-container">
    <div class="island-header">
      <h1>🏝️ 海岛生存</h1>
      <p>在荒岛上建立你的生存基地</p>
    </div>

    <div class="island-main">
      <div class="stats-panel">
        <div class="stat-card">
          <div class="stat-icon">🍖</div>
          <div class="stat-content">
            <div class="stat-number">{{ resources.food }}</div>
            <div class="stat-label">食物</div>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-icon">💧</div>
          <div class="stat-content">
            <div class="stat-number">{{ resources.water }}</div>
            <div class="stat-label">淡水</div>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-icon">🪵</div>
          <div class="stat-content">
            <div class="stat-number">{{ resources.wood }}</div>
            <div class="stat-label">木材</div>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-icon">⛏️</div>
          <div class="stat-content">
            <div class="stat-number">{{ resources.stone }}</div>
            <div class="stat-label">石头</div>
          </div>
        </div>

        <div class="stat-card stat-card--processed">
          <div class="stat-icon">⚙️</div>
          <div class="stat-content">
            <div class="stat-number">{{ resources.parts }}</div>
            <div class="stat-label">零件</div>
          </div>
        </div>

        <div class="stat-card stat-card--processed">
          <div class="stat-icon">🔥</div>
          <div class="stat-content">
            <div class="stat-number">{{ resources.fuel }}</div>
            <div class="stat-label">燃料</div>
          </div>
        </div>

        <div class="stat-card stat-card--processed">
          <div class="stat-icon">🛡️</div>
          <div class="stat-content">
            <div class="stat-number">{{ resources.armor }}</div>
            <div class="stat-label">防具</div>
          </div>
        </div>
      </div>

      <div class="capacity-bar">
        <div class="capacity-label">🎒 背包容量: {{ totalResources }} / {{ MAX_CAPACITY }}</div>
        <div class="capacity-track">
          <div class="capacity-fill" :style="{ width: capacityPercent + '%' }" :class="{ 'capacity-fill--warn': capacityPercent > 80, 'capacity-fill--full': capacityPercent >= 100 }"></div>
        </div>
      </div>

      <div class="actions-panel">
        <h3>📋 可执行操作</h3>

        <div class="action-grid">
          <div class="action-card" @click="gatherFood">
            <div class="action-icon">🍓</div>
            <div class="action-title">采集食物</div>
            <div class="action-desc">在岛上寻找可食用的果实和动物</div>
            <div class="action-time">耗时: 30秒</div>
          </div>

          <div class="action-card" @click="collectWater">
            <div class="action-icon">💧</div>
            <div class="action-title">收集淡水</div>
            <div class="action-desc">收集雨水或净化海水</div>
            <div class="action-time">耗时: 1分钟</div>
          </div>

          <div class="action-card" @click="chopWood">
            <div class="action-icon">🪓</div>
            <div class="action-title">砍伐木材</div>
            <div class="action-desc">砍伐树木获取木材资源</div>
            <div class="action-time">耗时: 2分钟</div>
          </div>

          <div class="action-card" @click="mineStone">
            <div class="action-icon">⛏️</div>
            <div class="action-title">挖掘石头</div>
            <div class="action-desc">在岛上挖掘石头资源</div>
            <div class="action-time">耗时: 3分钟</div>
          </div>

          <div class="action-card" @click="buildShelter">
            <div class="action-icon">🏠</div>
            <div class="action-title">建造庇护所</div>
            <div class="action-desc">建造一个安全的住所</div>
            <div class="action-cost">需要: 30木材, 20石头, 2零件</div>
          </div>

          <div class="action-card" @click="craftTools">
            <div class="action-icon">🔨</div>
            <div class="action-title">制作工具</div>
            <div class="action-desc">制作更高效的生存工具</div>
            <div class="action-cost">需要: 10木材, 5石头, 1零件</div>
          </div>
        </div>
      </div>

      <div class="process-panel">
        <h3>⚒️ 加工坊</h3>
        <p class="process-hint">将基础资源加工为高级物资</p>

        <div class="action-grid">
          <div class="action-card action-card--process" @click="craftFuel">
            <div class="action-icon">🔥</div>
            <div class="action-title">提炼燃料</div>
            <div class="action-desc">将木材提炼为燃料</div>
            <div class="action-cost">消耗: 10木材</div>
            <div class="action-gain">产出: 5燃料</div>
            <div class="action-time">耗时: 1分钟</div>
          </div>

          <div class="action-card action-card--process" @click="craftParts">
            <div class="action-icon">⚙️</div>
            <div class="action-title">打造零件</div>
            <div class="action-desc">用木材和石头制作精密零件</div>
            <div class="action-cost">消耗: 15木材, 10石头</div>
            <div class="action-gain">产出: 3零件</div>
            <div class="action-time">耗时: 1.5分钟</div>
          </div>

          <div class="action-card action-card--process" @click="craftArmor">
            <div class="action-icon">🛡️</div>
            <div class="action-title">锻造防具</div>
            <div class="action-desc">用石头和木材锻造防护装备</div>
            <div class="action-cost">消耗: 10木材, 20石头</div>
            <div class="action-gain">产出: 2防具</div>
            <div class="action-time">耗时: 2分钟</div>
          </div>
        </div>
      </div>

      <div class="map-panel">
        <h3>🗺️ 海岛地图</h3>
        <div class="map-container">
          <div class="map-grid">
            <div v-for="(cell, index) in mapGrid" :key="index"
                 :class="'map-cell ' + cell.type"
                 @click="exploreCell(index)">
              {{ cell.icon }}
            </div>
          </div>
          <div class="map-legend">
            <div class="legend-item">
              <span class="legend-icon">🌳</span>
              <span>森林</span>
            </div>
            <div class="legend-item">
              <span class="legend-icon">🏔️</span>
              <span>山地</span>
            </div>
            <div class="legend-item">
              <span class="legend-icon">🌊</span>
              <span>海洋</span>
            </div>
            <div class="legend-item">
              <span class="legend-icon">🏠</span>
              <span>营地</span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="message-log">
      <h3>📜 生存日志</h3>
      <div class="log-list">
        <div v-for="(msg, index) in messageLog" :key="index" class="log-item">
          <span class="log-time">{{ msg.time }}</span>
          <span class="log-content">{{ msg.content }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';
import { ElMessage, ElMessageBox } from 'element-plus';

const MAX_CAPACITY = 500;

const RESOURCE_NAMES = {
  food: '食物',
  water: '淡水',
  wood: '木材',
  stone: '石头',
  parts: '零件',
  fuel: '燃料',
  armor: '防具'
};

const resources = ref({
  food: 100,
  water: 100,
  wood: 100,
  stone: 100,
  parts: 0,
  fuel: 0,
  armor: 0
});

const totalResources = computed(() => {
  return Object.values(resources.value).reduce((sum, v) => sum + v, 0);
});

const capacityPercent = computed(() => {
  return Math.min((totalResources.value / MAX_CAPACITY) * 100, 100);
});

const messageLog = ref([
  { time: '00:00', content: '你来到了一个荒岛，开始你的生存之旅吧！' }
]);

const mapGrid = ref([
  { type: 'forest', icon: '🌳', explored: true },
  { type: 'forest', icon: '🌳', explored: true },
  { type: 'mountain', icon: '🏔️', explored: false },
  { type: 'ocean', icon: '🌊', explored: false },
  { type: 'camp', icon: '🏠', explored: true },
  { type: 'forest', icon: '🌳', explored: false },
  { type: 'ocean', icon: '🌊', explored: false },
  { type: 'mountain', icon: '🏔️', explored: false },
  { type: 'forest', icon: '🌳', explored: false }
]);

const addMessage = (content) => {
  const now = new Date();
  const time = `${now.getHours().toString().padStart(2, '0')}:${now.getMinutes().toString().padStart(2, '0')}`;
  messageLog.value.push({ time, content });
  if (messageLog.value.length > 20) {
    messageLog.value.shift();
  }
};

const formatCost = (cost) => {
  return Object.entries(cost)
    .map(([k, v]) => `${v}${RESOURCE_NAMES[k]}`)
    .join('、');
};

const canAfford = (cost) => {
  return Object.entries(cost).every(([res, amt]) => resources.value[res] >= amt);
};

const consumeResources = (cost) => {
  if (!canAfford(cost)) {
    const missing = Object.entries(cost)
      .filter(([res, amt]) => resources.value[res] < amt)
      .map(([res, amt]) => `${RESOURCE_NAMES[res]}(需${amt}有${resources.value[res]})`)
      .join('、');
    ElMessage.error(`资源不足: ${missing}`);
    return false;
  }
  for (const [res, amt] of Object.entries(cost)) {
    resources.value[res] -= amt;
  }
  return true;
};

const addResources = (gain) => {
  const totalGain = Object.values(gain).reduce((s, v) => s + v, 0);
  if (totalResources.value + totalGain > MAX_CAPACITY) {
    ElMessage.warning('背包容量不足，部分资源可能无法获取');
  }
  for (const [res, amt] of Object.entries(gain)) {
    resources.value[res] += amt;
  }
  return true;
};

const performAction = (name, cost, gain, time) => {
  if (!consumeResources(cost)) return false;

  addMessage(`开始${name}，消耗了${formatCost(cost)}...`);

  setTimeout(() => {
    addResources(gain);
    const gainText = Object.entries(gain).map(([k, v]) => `${v}${RESOURCE_NAMES[k]}`).join('、');
    addMessage(`${name}完成！获得了${gainText}`);
    ElMessage.success(`${name}完成！`);
  }, time);

  return true;
};

const gatherFood = () => {
  performAction('采集食物', {}, { food: 20 }, 30000);
};

const collectWater = () => {
  performAction('收集淡水', {}, { water: 30 }, 60000);
};

const chopWood = () => {
  performAction('砍伐木材', {}, { wood: 15 }, 120000);
};

const mineStone = () => {
  performAction('挖掘石头', {}, { stone: 10 }, 180000);
};

const buildShelter = () => {
  if (performAction('建造庇护所', { wood: 30, stone: 20, parts: 2 }, {}, 300000)) {
    addMessage('庇护所建造完成！你现在有了一个安全的住所。');
  }
};

const craftTools = () => {
  if (performAction('制作工具', { wood: 10, stone: 5, parts: 1 }, {}, 120000)) {
    addMessage('工具制作完成！你的工作效率提高了。');
  }
};

const craftFuel = () => {
  performAction('提炼燃料', { wood: 10 }, { fuel: 5 }, 60000);
};

const craftParts = () => {
  performAction('打造零件', { wood: 15, stone: 10 }, { parts: 3 }, 90000);
};

const craftArmor = () => {
  performAction('锻造防具', { wood: 10, stone: 20 }, { armor: 2 }, 120000);
};

const exploreCell = (index) => {
  const cell = mapGrid.value[index];
  if (cell.explored) {
    ElMessage.info('这个区域已经探索过了');
    return;
  }

  ElMessageBox.confirm(
    `确定要探索这个区域吗？可能会遇到危险或发现资源。`,
    '探索未知区域',
    {
      confirmButtonText: '开始探索',
      cancelButtonText: '取消',
      type: 'warning'
    }
  ).then(() => {
    addMessage(`开始探索${cell.icon}区域...`);

    setTimeout(() => {
      cell.explored = true;

      const random = Math.random();
      if (random < 0.3) {
        const foodGain = Math.floor(Math.random() * 20) + 10;
        addResources({ food: foodGain });
        addMessage(`探索发现了食物！获得${foodGain}食物`);
        ElMessage.success(`探索发现了食物！获得${foodGain}食物`);
      } else if (random < 0.6) {
        const woodGain = Math.floor(Math.random() * 15) + 5;
        addResources({ wood: woodGain });
        addMessage(`探索发现了木材！获得${woodGain}木材`);
        ElMessage.success(`探索发现了木材！获得${woodGain}木材`);
      } else if (random < 0.8) {
        const stoneGain = Math.floor(Math.random() * 10) + 5;
        addResources({ stone: stoneGain });
        addMessage(`探索发现了石头！获得${stoneGain}石头`);
        ElMessage.success(`探索发现了石头！获得${stoneGain}石头`);
      } else {
        consumeResources({ food: 10, water: 10 });
        addMessage(`探索遇到了危险！损失了10食物和10水`);
        ElMessage.warning(`探索遇到了危险！损失了10食物和10水`);
      }
    }, 5000);
  }).catch(() => {
    addMessage('取消了探索');
  });
};

const resetGame = () => {
  resources.value = {
    food: 100,
    water: 100,
    wood: 100,
    stone: 100,
    parts: 0,
    fuel: 0,
    armor: 0
  };
  addMessage('重新开始游戏！');
};

onMounted(() => {
  addMessage('欢迎来到海岛生存游戏！');
  setInterval(() => {
    if (!consumeResources({ food: 5, water: 5 })) {
      ElMessageBox.alert(
        '你的食物或水耗尽了，游戏结束！',
        '游戏结束',
        {
          confirmButtonText: '重新开始',
          type: 'error'
        }
      ).then(() => {
        resetGame();
      });
    }
  }, 60000);
});
</script>

<style scoped>
.island-container {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  padding: 20px;
}

.island-header {
  text-align: center;
  color: white;
  margin-bottom: 30px;
}

.island-header h1 {
  font-size: 48px;
  margin: 0 0 10px 0;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}

.island-header p {
  font-size: 18px;
  margin: 0;
  opacity: 0.9;
}

.island-main {
  max-width: 1200px;
  margin: 0 auto;
}

.stats-panel {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
  gap: 16px;
  margin-bottom: 20px;
}

.stat-card {
  background: white;
  border-radius: 12px;
  padding: 16px;
  display: flex;
  align-items: center;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.stat-card--processed {
  background: linear-gradient(135deg, #fff7ed 0%, #fef3c7 100%);
  border: 1px solid #f59e0b;
}

.stat-icon {
  font-size: 36px;
  margin-right: 12px;
}

.stat-content {
  flex: 1;
}

.stat-number {
  font-size: 28px;
  font-weight: bold;
  color: #333;
  margin-bottom: 2px;
}

.stat-label {
  font-size: 13px;
  color: #666;
}

.capacity-bar {
  background: white;
  border-radius: 12px;
  padding: 16px 20px;
  margin-bottom: 20px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.capacity-label {
  font-size: 14px;
  font-weight: bold;
  color: #333;
  margin-bottom: 8px;
}

.capacity-track {
  width: 100%;
  height: 12px;
  background: #e5e7eb;
  border-radius: 6px;
  overflow: hidden;
}

.capacity-fill {
  height: 100%;
  background: linear-gradient(90deg, #10b981, #34d399);
  border-radius: 6px;
  transition: width 0.4s ease;
}

.capacity-fill--warn {
  background: linear-gradient(90deg, #f59e0b, #fbbf24);
}

.capacity-fill--full {
  background: linear-gradient(90deg, #ef4444, #f87171);
}

.actions-panel {
  background: white;
  border-radius: 12px;
  padding: 30px;
  margin-bottom: 20px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.actions-panel h3 {
  margin: 0 0 20px 0;
  font-size: 24px;
  color: #333;
}

.process-panel {
  background: white;
  border-radius: 12px;
  padding: 30px;
  margin-bottom: 20px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  border-left: 4px solid #f59e0b;
}

.process-panel h3 {
  margin: 0 0 4px 0;
  font-size: 24px;
  color: #333;
}

.process-hint {
  margin: 0 0 20px 0;
  font-size: 14px;
  color: #999;
}

.action-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 20px;
}

.action-card {
  background: #f8f9fa;
  border-radius: 8px;
  padding: 20px;
  cursor: pointer;
  transition: all 0.3s ease;
  border: 2px solid transparent;
}

.action-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
  border-color: #667eea;
}

.action-card--process {
  background: linear-gradient(135deg, #fffbeb 0%, #fef3c7 100%);
}

.action-card--process:hover {
  border-color: #f59e0b;
}

.action-icon {
  font-size: 40px;
  margin-bottom: 10px;
}

.action-title {
  font-size: 18px;
  font-weight: bold;
  color: #333;
  margin-bottom: 6px;
}

.action-desc {
  font-size: 14px;
  color: #666;
  margin-bottom: 8px;
}

.action-time,
.action-cost {
  font-size: 12px;
  color: #999;
}

.action-gain {
  font-size: 12px;
  color: #16a34a;
  font-weight: bold;
}

.map-panel {
  background: white;
  border-radius: 12px;
  padding: 30px;
  margin-bottom: 30px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.map-panel h3 {
  margin: 0 0 20px 0;
  font-size: 24px;
  color: #333;
}

.map-container {
  text-align: center;
}

.map-grid {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  gap: 10px;
  justify-content: center;
  margin-bottom: 30px;
}

.map-cell {
  width: 100px;
  height: 100px;
  background: #f0f0f0;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 40px;
  cursor: pointer;
  transition: all 0.3s ease;
  border: 2px solid #ddd;
}

.map-cell:hover {
  transform: scale(1.05);
  border-color: #667eea;
}

.map-cell.explored {
  background: #e8f4fa;
  border-color: #409eff;
}

.map-legend {
  display: flex;
  justify-content: center;
  gap: 30px;
  flex-wrap: wrap;
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 14px;
}

.legend-icon {
  font-size: 24px;
}

.message-log {
  background: white;
  border-radius: 12px;
  padding: 30px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.message-log h3 {
  margin: 0 0 20px 0;
  font-size: 24px;
  color: #333;
}

.log-list {
  max-height: 300px;
  overflow-y: auto;
  border: 1px solid #eee;
  border-radius: 8px;
  padding: 10px;
}

.log-item {
  display: flex;
  margin-bottom: 8px;
  padding: 8px;
  background: #f8f9fa;
  border-radius: 4px;
}

.log-time {
  font-weight: bold;
  color: #409eff;
  margin-right: 12px;
  min-width: 60px;
}

.log-content {
  flex: 1;
  color: #666;
}

@media (max-width: 768px) {
  .island-header h1 {
    font-size: 32px;
  }

  .stats-panel {
    grid-template-columns: repeat(2, 1fr);
  }

  .action-grid {
    grid-template-columns: 1fr;
  }
}
</style>
