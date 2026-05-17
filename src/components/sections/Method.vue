<script lang="ts">
import { defineComponent } from 'vue'
import { VueLatex } from 'vatex'

export default defineComponent({
  components: {
    VueLatex
  }
})
</script>

<template>
  <div>
    <el-divider />

    <el-row justify="center">
      <h1 class="section-title">Method</h1>
    </el-row>

    <!-- method figure -->
    <el-row justify="center">
      <el-col :xs="24" :sm="22" :md="20" :lg="18" :xl="16">
        <img class="method-img" src="/figs/pipeline.png" alt="Demo-JEPA Pipeline" />
      </el-col>
    </el-row>

    <el-row justify="center">
      <el-col :xs="24" :sm="20" :md="18" :lg="15" :xl="14">

        <div class="method-block">
          <h2>Overview</h2>
          <p>
            Demo-JEPA formulates cross-embodiment imitation as latent goal-conditioned planning.
            Given a source demonstration and the current target observation, it first predicts a
            target-compatible future latent goal, and then plans target actions toward this goal
            with an action-conditioned JEPA world model.
          </p>

          <p>
            This design avoids direct action alignment across different robots. Instead, Demo-JEPA
            transfers the demonstrated intent in latent space and lets the target embodiment realize
            it through its own dynamics.
          </p>
        </div>

        <div class="method-block">
          <h2>Dreamer Predictor</h2>
          <p>
            The Dreamer Predictor is the core module of Demo-JEPA. It maps the current target
            observation and a source motion segment into JEPA latents, then predicts a future latent
            goal for the target embodiment.
          </p>

          <p class="formula">
            <VueLatex
              expression="(o_k^t, o_k^s, o_{k+n}^s) \xrightarrow{E(\cdot)} (z_k^t, z_k^s, z_{k+n}^s)"
              display-mode
            />
          </p>

          <p>
            The predictor models two key factors: embodiment correspondence and demonstration
            motion. Two cross-attention modules extract these factors from the source and target
            latents.
          </p>

          <p class="formula">
            <VueLatex
              expression="f_{\mathrm{emb}}=\mathrm{Attn}(Q=z_k^t,K=z_k^s,V=z_k^s)"
              display-mode
            />
          </p>

          <p class="formula">
            <VueLatex
              expression="f_{\mathrm{mot}}=\mathrm{Attn}(Q=z_{k+n}^s,K=z_k^s,V=z_k^s)"
              display-mode
            />
          </p>

          <p>
            The target latent, embodiment-aware feature, and motion-aware feature are fused by a 3D
            convolutional block. A transformer predictor then decodes the fused representation into
            the target-compatible latent goal.
          </p>

          <p class="formula">
            <VueLatex
              expression="f_{\mathrm{fused}}=\phi([z_k^t \oplus f_{\mathrm{emb}} \oplus f_{\mathrm{mot}}])"
              display-mode
            />
          </p>

          <p class="formula">
            <VueLatex
              expression="\hat{z}_{\mathrm{goal}}^t=\mathcal{T}(f_{\mathrm{fused}})"
              display-mode
            />
          </p>
        </div>

        <div class="method-block">
          <h2>Training Procedure</h2>

          <div class="stage-card">
            <h3>Stage 0: Target World Model Initialization</h3>
            <p>
              Demo-JEPA first initializes an action-conditioned JEPA world model on target robot
              trajectories, learning latent dynamics conditioned on robot states and actions.
            </p>

            <p class="formula">
              <VueLatex
                expression="\hat{z}_{k+1}=F_{\mathrm{wm}}(z_k^t,s_k^t,a_k^t)"
                display-mode
              />
            </p>
          </div>

          <div class="stage-card">
            <h3>Stage 1: Latent Goal Predictor Training</h3>
            <p>
              The Dreamer Predictor is trained to translate source motion into target-compatible
              latent goals. Its prediction is supervised by the encoded future latent of the target
              embodiment.
            </p>

            <p class="formula">
              <VueLatex
                expression="\mathcal{L}_{\mathrm{pred}}=\left\|\hat{z}_{\mathrm{goal}}^t-z_{k+n}^t\right\|_2^2"
                display-mode
              />
            </p>

            <p>
              Temporal perturbation is used to improve robustness under imperfect alignment and
              planning noise.
            </p>

            <p class="formula">
              <VueLatex
                expression="(o_k^s,o_{k+n}^s) \rightarrow (o_{k+\delta}^s,o_{k+n}^s),\quad \delta\sim\mathcal{U}(-r,r)"
                display-mode
              />
            </p>
          </div>

          <div class="stage-card">
            <h3>Stage 2: Action Co-Training</h3>
            <p>
              After the Dreamer Predictor is frozen, the world-model dynamics predictor is further
              optimized so that action-conditioned rollouts align with Dreamer-predicted latent goals.
            </p>

            <p class="formula">
              <VueLatex
                expression="\mathcal{L}_{\mathrm{plan}}=\left\|F_{\mathrm{wm}}(z_k^t,s_k^t,\mathbf{a}_{k:k+n-1}^t)-\hat{z}_{\mathrm{goal}}^t\right\|_2^2"
                display-mode
              />
            </p>
          </div>
        </div>

        <div class="method-block">
          <h2>Inference</h2>
          <p>
            During inference, each source demonstration is treated as a sequence of reference
            segments. For each segment, Demo-JEPA predicts a latent goal and uses CEM to optimize
            target actions whose latent rollout reaches that goal.
          </p>

          <p class="formula">
            <VueLatex
              expression="\mathbf{a}_{k:k+H-1}^{t*}=\arg\min_{\mathbf{a}} d(F_{\mathrm{wm}}(z_k^t,s_k^t,\mathbf{a}),z_{\mathrm{goal}}^t)"
              display-mode
            />
          </p>

          <p>
            To stabilize long-horizon execution, Demo-JEPA advances to the next source segment only
            when the current latent goal has been reached. Otherwise, the same goal remains active
            and the planner continues optimizing toward it.
          </p>

          <p class="formula">
            <VueLatex
              expression="D_k=d(z_{k+1}^t,\hat{z}_{\mathrm{goal}}^t),\quad \mathrm{advance\ if}\ D_k<\epsilon"
              display-mode
            />
          </p>
        </div>

      </el-col>
    </el-row>
  </div>
</template>

<style scoped>
.method-img {
  width: 100%;
  display: block;
  margin: 20px auto 32px auto;
}

.method-block {
  margin: 34px 0;
}

.method-block h2 {
  font-size: 22px;
  font-weight: 600;
  margin: 0 0 14px 0;
}

.method-block p {
  font-size: 16px;
  line-height: 1.75;
  text-align: justify;
  margin: 10px 0;
}

.stage-card {
  margin: 18px 0;
  padding: 18px 22px;
  border-radius: 12px;
  background: #fafafa;
  border: 1px solid #eeeeee;
}

.stage-card h3 {
  font-size: 18px;
  font-weight: 600;
  margin: 0 0 10px 0;
}

.formula {
  text-align: center;
  margin: 16px 0;
  overflow-x: auto;
}

@media (max-width: 768px) {
  .method-block h2 {
    font-size: 19px;
  }

  .stage-card h3 {
    font-size: 16px;
  }

  .method-block p {
    font-size: 15px;
    line-height: 1.65;
    text-align: left;
  }

  .stage-card {
    padding: 14px 16px;
  }
}
</style>