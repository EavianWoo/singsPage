<template>
  <div>

    <el-divider />

    <!-- 标题 -->
    <el-row justify="center">
        <h1 class="section-title">Detail Discussion</h1>
    </el-row>

    <el-row justify="center">
        <el-col :xs="24" :sm="20" :md="16" :lg="12" :xl="12">

            <!-- 折叠列表 -->
            <el-collapse model-value="1" class="collapse" accordion >

                <!-- 第一个折叠列表信息 -->
                <el-collapse-item title="Consistency" name="1">
                    <div style="text-align: justify; width: 100%;">
                      Unlike these methods that depends on SDS to simply samples in static camera space, SinGS brings motion to the stage with <strong>KHD</strong> that samples directly in <strong>kinematic human space</strong>. This allows the us to effectively leverage readily available monocular multi-motion videos, and to incorporate temporal information as a more powerful consistency constraint - which has been shown to improve identity consistency.
                    </div>
                </el-collapse-item>

                <el-collapse-item title="Quality" name="2">
                    <div>
                      To ensure high-quality and structurally consistent reconstructions, <strong>GPGS</strong> integrates
                      <div style="text-align: justify; width: 100%;">
                        - <em>Semantic Laplacian Regularization</em> which provides semantic region-specific control by relaxing restrictions in expressive areas (e.g. head) to preserve fine-grained details while tightening elsewhere to ensure surface coherence and geometric smoothness.
                      </div>
                      <div style="text-align: justify; width: 100%;">
                        - <em>Geometry-Preserving Density Control</em> which prevents over-pruning or uncontrolled growth of Gaussians.
                      </div>
                      <div style="text-align: justify; width: 100%;">
                        These strategies, combined with mesh-guided optimization and a triplane-based network, enable <strong>GPGS</strong> to produce accurate and animatable human avatar even under imperfect priors.
                      </div>
                    </div>
                </el-collapse-item>

                <el-collapse-item title="Efficiency" name="3">
                    <div>
                        To be updated;
                    </div>
                </el-collapse-item>

                <el-collapse-item title="Animation" name="4">
                    <div>
                        To be updated;
                    </div>
                </el-collapse-item>
            </el-collapse>
        </el-col>
    </el-row>

  </div>
</template>
  
<script>
</script>

<style scoped>
.collapse {
    margin-top: 20px;
}
</style>
