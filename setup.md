

## 激活环境

source .venv/bin/activate



## 测试指令

### 测试mujoco环境
python python/mujoco/specs_test.py
python -m mujoco.viewer --mjcf /Users/cengguangjun/Desktop/OpenDriveLab/mujoco/model/humanoid/humanoid.xml

### 测试动力学辨识

pytest -q python/mujoco/sysid/tests/test_integration.py::test_arm_recover_armature -s
pytest -q python/mujoco/sysid/tests/test_integration.py::test_spring_mass_recover_mass -s

  iter: k  y: ...  dx: ... reduction: ... 分别代表什么
  这是 MuJoCo 自带的 least-squares 优化器（mujoco.minimize.least_squares）每次迭代的日
  志：
  - iter: k：第 k 次迭代
  - y：当前目标值（本质上是 (|r(x)|^2) 这一类的平方误差；越小越好）
  - dx：本次更新步长（参数向量变化幅度的量级）；后期接近收敛会变很小
  - reduction：这一迭代让 y 降低了多少（下降量）
  - ratio：实际下降 / 预测下降 的比值，用来判断这步是不是“靠谱”
  - log10mu：阻尼/信赖域相关的内部参数（这里显示 -inf 属于实现细节，不影响你理解“在收
    敛”）
  终止行：
  - Terminated after ... iterations: norm(gradient) < tol.：梯度范数小于阈值，认为收敛
  - Residual evals: 65：总共调用 residual 函数 65 次（有限差分雅可比会导致每轮多次
    rollout，所以 eval 次数通常 > 迭代次数）
  - residual 97.9%：绝大部分时间花在 rollout+计算 residual 上（很正常）