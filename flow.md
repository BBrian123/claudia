Git Fork 同步工作流：保持与上游项目更新本文档旨在为您提供一个清晰、规范的 Git 工作流程，用于在对自己 Fork 的项目进行修改的同时，能够持续、干净地从原始（上游）项目获取更新。核心目标独立开发：在你自己的项目副本（Fork）上自由进行修改和实验。保持同步：定期将原始项目的最新代码合并到你的副本中。提交历史清晰：保持一个线性的、易于理解的提交记录，为将来向上游项目贡献代码（创建 Pull Request）做好准备。一、一次性设置对于每个项目，以下设置只需操作一次。1. Fork 原始项目在 GitHub 页面上，将原始项目（例如 getAsterisk/claudia）Fork到你自己的账户下。这将创建属于你的项目副本（例如 YOUR_USERNAME/claudia）。2. 克隆你的 Fork 到本地将你自己的 Fork 克隆到本地计算机。# 将 YOUR_USERNAME 替换为你的 GitHub 用户名
git clone git@github.com:YOUR_USERNAME/claudia.git
cd claudia
3. 添加“上游”远端 (upstream)为你的本地仓库添加一个指向原始项目的远端，我们通常将其命名为 upstream。# 将 URL 替换为原始项目的 URL
git remote add upstream https://github.com/getAsterisk/claudia.git
4. 验证配置运行以下命令，检查远端配置是否正确。你应该能同时看到 origin（指向你的 Fork）和 upstream（指向原始项目）。git remote -v
预期输出：origin    git@github.com:YOUR_USERNAME/claudia.git (fetch)
origin    git@github.com:YOUR_USERNAME/claudia.git (push)
upstream  https://github.com/getAsterisk/claudia.git (fetch)
upstream  https://github.com/getAsterisk/claudia.git (push)
二、日常开发与同步工作流这是一个周期性执行的流程，建议在每次开始新的开发工作前都操作一遍。第 1 步：获取上游更新从 upstream 远端下载最新的分支和提交信息。这是一个安全的操作，不会影响你本地的代码。git fetch upstream
第 2 步：合并更新 (Rebase)将你的本地修改“变基”到上游项目的最新版本之上。这能创造一个干净、线性的提交历史。首先，确保你位于要更新的本地分支（通常是 main 或 master）。git checkout main
然后，执行 rebase 命令。git rebase upstream/main
注意：处理冲突如果 rebase 过程中出现冲突（即你的修改和上游的更新动了同一处代码），Git 会暂停。根据提示，手动编辑并解决冲突文件。使用 git add <文件名> 将解决后的文件标记为已解决。运行 git rebase --continue 继续变基过程。如果想放弃本次变基，可随时运行 git rebase --abort。第 3 步：保存工作到你的 Fork当本地分支成功合并了上游的更新后，将最终结果推送到你自己的 GitHub 仓库 (origin)。由于 rebase 修改了你本地的提交历史，你需要使用带有安全检查的强制推送命令 --force-with-lease。git push --force-with-lease origin main
流程总结（快速参考）# 1. 获取更新
git fetch upstream

# 2. 切换到主分支
git checkout main

# 3. 变基到上游最新版本
git rebase upstream/main

# 4. 推送到自己的 Fork
git push --force-with-lease origin main
遵循此流程，您就可以高效地管理个人项目，并与社区保持同步。