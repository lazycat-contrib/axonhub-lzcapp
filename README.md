# axonhub-lzcapp

# 最简用法 —— 只需传版本号（单镜像，自动记忆配置）
  scripts/lzc-release-update.sh 1.0.0

# 指定源镜像（如果 ghcr.io 源不对）
  scripts/lzc-release-update.sh 1.0.0 --source-image docker.io/looplj/axonhub:1.0.0

# 更新 + 构建 + 发布到应用商店
  scripts/lzc-release-update.sh 1.0.0 --publish --changelog "升级到 1.0.0 正式版"

# 只更新文件不构建（调试用）
  scripts/lzc-release-update.sh 1.0.0 --skip-copy --skip-build
