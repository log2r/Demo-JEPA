<script lang="ts" setup>
type ResultRow = {
  method: string
  behavior: number | null
  bridging: number | null
  zeroShot: number | null
  note: string
}

type MetricKey = 'behavior' | 'bridging' | 'zeroShot'

const simResults: ResultRow[] = [
  {
    method: 'XSkill',
    behavior: 0.39,
    bridging: 0.17,
    zeroShot: 0.03,
    note: 'Skill-prototype transfer baseline',
  },
  {
    method: 'VPP',
    behavior: 0.47,
    bridging: 0.28,
    zeroShot: 0.04,
    note: 'Strong in behavior grounding',
  },
  {
    method: 'Demo-JEPA',
    behavior: 0.31,
    bridging: 0.45,
    zeroShot: 0.36,
    note: 'Best under larger distribution shifts',
  },
]

const realResults: ResultRow[] = [
  {
    method: 'XSkill',
    behavior: 0.45,
    bridging: 0.40,
    zeroShot: 0.05,
    note: 'Limited zero-shot transfer',
  },
  {
    method: 'VPP',
    behavior: 0.65,
    bridging: 0.53,
    zeroShot: 0.00,
    note: 'Best behavior grounding',
  },
  {
    method: 'Demo-JEPA',
    behavior: 0.43,
    bridging: 0.55,
    zeroShot: 0.25,
    note: 'Best transfer and zero-shot performance',
  },
]

const formatRate = (value: number | null) => {
  if (value === null) return '--'
  return `${Math.round(value * 100)}%`
}

const rowClassName = ({ row }: { row: ResultRow }) => {
  return row.method === 'Demo-JEPA' ? 'ours-row' : ''
}

const isBest = (row: ResultRow, key: MetricKey, data: ResultRow[]) => {
  const values = data
    .map(item => item[key])
    .filter((value): value is number => value !== null)

  if (row[key] === null || values.length === 0) return false

  return row[key] === Math.max(...values)
}
</script>

<template>
  <div>
    <el-divider />

    <el-row justify="center">
      <h1 class="section-title">Main Results</h1>
    </el-row>

    <el-row justify="center">
      <el-col :xs="24" :sm="22" :md="20" :lg="16" :xl="14">
        <p class="result-intro">
          We evaluate Demo-JEPA in both simulation and real-world cross-embodiment transfer.
          The target Franka policy is conditioned on held-out source demonstrations, and task
          success rate is reported across three evaluation suites with increasing distribution shift.
        </p>

        <el-card class="card">
          <el-tabs class="result-tabs" model-value="Simulation">

            <el-tab-pane label="Simulation" name="Simulation">
              <el-table
                :data="simResults"
                :row-class-name="rowClassName"
                scrollbar-always-on
              >
                <el-table-column prop="method" label="Method" min-width="130" sortable />

                <el-table-column prop="behavior" label="Behavior Grounding" min-width="170" sortable>
                  <template #default="{ row }">
                    <span :class="{ 'best-value': isBest(row, 'behavior', simResults) }">
                      {{ formatRate(row.behavior) }}
                    </span>
                  </template>
                </el-table-column>

                <el-table-column prop="bridging" label="Cross-Embodiment Bridging" min-width="210" sortable>
                  <template #default="{ row }">
                    <span :class="{ 'best-value': isBest(row, 'bridging', simResults) }">
                      {{ formatRate(row.bridging) }}
                    </span>
                  </template>
                </el-table-column>

                <el-table-column prop="zeroShot" label="Zero-Shot Generalization" min-width="210" sortable>
                  <template #default="{ row }">
                    <span :class="{ 'best-value': isBest(row, 'zeroShot', simResults) }">
                      {{ formatRate(row.zeroShot) }}
                    </span>
                  </template>
                </el-table-column>

                <el-table-column prop="note" label="Summary" min-width="260" />
              </el-table>

              <p class="result-text">
                In simulation, VPP performs best in behavior grounding, where the evaluation is
                closest to in-domain trajectory learning. As the distribution shift increases,
                Demo-JEPA becomes clearly stronger. It achieves the best average success rate in
                cross-embodiment bridging and zero-shot generalization, showing that the Dreamer
                Predictor can infer executable target-compatible latent goals beyond direct action
                supervision.
              </p>
            </el-tab-pane>

            <el-tab-pane label="Real World" name="Real World">
              <el-table
                :data="realResults"
                :row-class-name="rowClassName"
                scrollbar-always-on
              >
                <el-table-column prop="method" label="Method" min-width="130" sortable />

                <el-table-column prop="behavior" label="Behavior Grounding" min-width="170" sortable>
                  <template #default="{ row }">
                    <span :class="{ 'best-value': isBest(row, 'behavior', realResults) }">
                      {{ formatRate(row.behavior) }}
                    </span>
                  </template>
                </el-table-column>

                <el-table-column prop="bridging" label="Cross-Embodiment Bridging" min-width="210" sortable>
                  <template #default="{ row }">
                    <span :class="{ 'best-value': isBest(row, 'bridging', realResults) }">
                      {{ formatRate(row.bridging) }}
                    </span>
                  </template>
                </el-table-column>

                <el-table-column prop="zeroShot" label="Zero-Shot Generalization" min-width="210" sortable>
                  <template #default="{ row }">
                    <span :class="{ 'best-value': isBest(row, 'zeroShot', realResults) }">
                      {{ formatRate(row.zeroShot) }}
                    </span>
                  </template>
                </el-table-column>

                <el-table-column prop="note" label="Summary" min-width="260" />
              </el-table>

              <p class="result-text">
                The real-world results follow the same trend. VPP is strongest in behavior grounding,
                while Demo-JEPA becomes more effective under stronger distribution shifts. Demo-JEPA
                achieves the best reported average success rate in cross-embodiment bridging and
                substantially outperforms the baselines in zero-shot generalization.
              </p>
            </el-tab-pane>

          </el-tabs>
        </el-card>

        <div class="takeaway">
          <h2>Key Takeaway</h2>
          <p>
            Demo-JEPA is most effective when the test scenario moves away from the action-supervised
            training distribution. Rather than treating demonstrations as fixed action sequences or
            skill prototypes, it converts heterogeneous source demonstrations into latent goals that
            can be executed by the target embodiment through planning.
          </p>
        </div>

        <div class="more-details">
          <p>
            More details can be found in our
            <a href="#" target="_blank">paper</a>
            and
            <a href="#" target="_blank">code</a>.
          </p>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<style scoped>
.card {
  margin-top: 22px;
}

.result-intro {
  font-size: 16px;
  line-height: 1.75;
  text-align: justify;
  margin: 16px 0 20px 0;
}

.result-text {
  font-size: 15px;
  line-height: 1.7;
  text-align: justify;
  margin: 18px 2px 4px 2px;
}

.takeaway {
  margin: 28px 0 10px 0;
  padding: 20px 24px;
  border-radius: 12px;
  background: #fafafa;
  border: 1px solid #eeeeee;
}

.takeaway h2 {
  font-size: 21px;
  font-weight: 600;
  margin: 0 0 10px 0;
}

.takeaway p {
  font-size: 16px;
  line-height: 1.75;
  text-align: justify;
  margin: 0;
}

.more-details {
  margin: 22px 0 8px 0;
  text-align: center;
}

.more-details p {
  font-size: 20px;
  line-height: 1.7;
  margin: 0;
}

.more-details a {
  color: #409eff;
  text-decoration: none;
  font-weight: 600;
}

.more-details a:hover {
  text-decoration: underline;
}

/* 只高亮 Demo-JEPA 所在行，不加粗整行 */
:deep(.ours-row) {
  background-color: #f7fbff;
}

/* 每一列最大值单独加粗 */
.best-value {
  font-weight: 700;
}

@media (max-width: 768px) {
  .result-intro,
  .result-text,
  .takeaway p {
    font-size: 15px;
    line-height: 1.65;
    text-align: left;
  }

  .more-details p {
    font-size: 18px;
    line-height: 1.65;
    text-align: left;
  }

  .takeaway {
    padding: 16px 18px;
  }

  .takeaway h2 {
    font-size: 18px;
  }

  .more-details {
    text-align: left;
  }
}
</style>