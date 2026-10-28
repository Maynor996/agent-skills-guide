> 在线阅读（GitHub Pages）：https://maynor996.github.io/agent-skills-guide/

# 让 AI 编程助手学会做视频、做 PPT：Agent Skills 入门与安装全指南

### 一、Agent Skills 是什么

用 Claude Code、Codex、Cursor 这类编码 Agent 时，你可能遇到过这种情况：  
同一个任务（比如"做一张文章封面"或"把更新日志做成短视频"），每次都要重新写一大段提示词，  
效果还时好时坏。

**Agent Skill 就是为了解决这个问题。** 简单说，一个 Skill 就是一个文件夹，里面至少有一个 `SKILL.md`：
    
    
    my-skill/
    ├── SKILL.md        # 说明：这个技能做什么、什么时候用、按什么步骤做
    ├── scripts/        # 可选：辅助脚本
    └── assets/         # 可选：模板、素材
    

`SKILL.md` 开头的元信息里写着技能名称和用途。Agent 平时只读这一小段，  
等你的任务和某个技能匹配了，才会去加载完整的说明和脚本。  
你可以把它理解成"给 Agent 的岗位 SOP"：写一次，以后反复用。

现在支持 SKILL.md 标准的 Agent 有 Claude Code、Codex、Cursor、Gemini CLI、OpenCode 等。

### 二、安装到哪里

不同 Agent 读取技能的目录不一样：

Agent| 用户级技能目录  
---|---  
Claude Code| `~/.claude/skills/`  
Codex| `~/.agents/skills/`  
Cursor| `~/.agents/skills/`  
  
#### 方式 1：手动安装（最透明）

以 GitHub 上的某个技能仓库为例：
    
    
    # 1. 克隆仓库
    git clone https://github.com/<作者>/<仓库>.git /tmp/skill-repo
    
    # 2. 把技能文件夹复制到对应目录（以 Claude Code 为例）
    mkdir -p ~/.claude/skills
    cp -r /tmp/skill-repo/skills/<技能名> ~/.claude/skills/
    
    # Codex / Cursor 用户换成：
    # mkdir -p ~/.agents/skills && cp -r /tmp/skill-repo/skills/<技能名> ~/.agents/skills/
    

装完后**重启一次 Agent 会话** ，然后直接用自然语言描述任务就行，不用专门去"调用"它。

#### 方式 2：让 Agent 自己装

直接对 Agent 说：“帮我把 https://github.com/xxx/yyy 里的 zzz 技能安装到我的 skills 目录”，  
它会自己完成克隆和复制。我自己常用 SkillHub（https://skills-hub.chatgpt-plus.workers.dev/zh）这个目录站找技能，  
它的每个详情页都有一键复制的安装提示词，复制后粘贴给 Agent 就行，省得自己拼路径。

> 安全提示：Skill 可能带脚本，安装前最好看一眼 SKILL.md 和 scripts，只装来源清楚的技能。

### 三、3 个推荐技能和使用步骤

下面 3 个都是开源免费的，用之前请看一下各自仓库的许可证。

#### 推荐 1：Humanizer-zh（歸藏 op7418）—— 去掉中文文案的"AI 味"

**适合** ：写公众号、技术文章、产品文案，嫌 AI 写出来的东西太套路的人。

步骤：

  1. 克隆 `op7418/Humanizer-zh`，把技能文件夹复制到 `~/.claude/skills/`（Codex / Cursor 复制到 `~/.agents/skills/`）。
  2. 重启会话，把一段 AI 生成的文字发给 Agent：“帮我把这段改得更像人写的，保留原意。”
  3. 对比改写前后的版本，根据需要再让它调整语气。

#### 推荐 2：baoyu-skills（宝玉 JimLiu）—— 内容创作工具箱

**适合** ：经常要做文章配图、封面、内容排版的写作者。

步骤：

  1. 克隆 `JimLiu/baoyu-skills`。仓库里有多个技能，按需挑选，复制到 skills 目录。
  2. 看一下每个技能的 SKILL.md，确认需要准备什么（有的技能需要配置图像生成的 API）。
  3. 对 Agent 说出具体需求，比如"给这篇文章做一张封面图"，Agent 会匹配到对应技能。

#### 推荐 3：huashu-design（花叔 alchaincyf）—— 设计类产出

**适合** ：想让 Agent 做出更有设计感的页面、视觉稿的开发者。

步骤：

  1. 克隆 `alchaincyf/huashu-design`，复制技能文件夹到 skills 目录。
  2. 重启会话，描述需求：“做一个产品介绍页，风格简洁、偏杂志感。”
  3. 在 Agent 给出的初稿上继续迭代。想做动效的话，可以再试试同一作者的 `huashu-art-motion`。

### 四、怎么找到更多好用的技能

技能多了以后，最麻烦的是**不知道装上能得到什么** 。我的经验：

  1. **先看产出样例再装** ：选有成品示例的技能。
  2. **看"适合谁 / 不适合谁"** ：别把一堆用不上的技能全装上，会干扰 Agent 匹配。
  3. **认准许可证和原作者** ：优先选 MIT / Apache-2.0 这类宽松许可证的仓库。

SkillHub 就是按这个思路整理的：67 个技能，可以按视频 / 图片 / 幻灯片 / 网页 / 文档筛选，  
每个页面写了产出内容、适用人群、前置条件和原作者 / 许可证。还有 22 条从 X 帖子整理的  
精选提示词，附原帖链接。收录的开源技能都免费，作者自研的 10 个视频 / 图片模板技能是付费的 Pro，有需要再看。

### 五、小结

  * Skill = 一个带 SKILL.md 的文件夹，相当于给 Agent 的可复用 SOP；
  * Claude Code 装到 `~/.claude/skills/`，Codex / Cursor 装到 `~/.agents/skills/`；
  * 先看效果、看许可证、按需安装。

有问题欢迎评论区交流，你常用哪些技能也欢迎推荐。