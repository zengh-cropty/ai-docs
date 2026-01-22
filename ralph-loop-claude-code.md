# 让 Claude Code 彻夜干活：ralph loop 插件完全指南

过去一个月，Claude Code 之父 Boris 提交了 259 个 PR，添加了 40000 行代码。

这不是他熬夜的结果。背后的秘密，是 Claude Code 不眠不休地在干活。

今天给大家介绍的，就是让这一切成为可能的秘密武器——ralph loop 插件。

## 什么是 ralph loop

ralph loop 这个名字，来源于《辛普森一家》里的动画角色 Ralph Wiggum。这个角色的特点是什么？智力不算高，但有一种"天真执着"的劲头。认定了目标，就算犯错也会一直尝试，直到成功。

去年，一位澳洲开发者 Geoffrey Huntley 最早提出了这个理念。他写了一个只有 5 行的 Bash 脚本，核心思想非常简单：如果需求没做完，就继续循环做。

别扯什么"人在回路"（Human in the loop），让 AI 自己跑就行。

Anthropic 最近把这个理念做成了官方插件，集成进了 Claude Code。由于版权原因，插件从 ralph-wiggum 改名为 ralph-loop，但核心能力完全一样。

## ralph loop 适合什么场景

使用 AI 编程工具时，你是不是经常遇到这种情况：一个任务开始跑后，得时不时去检查进度，再给它一些反馈。就算你说"跑完所有测试，中间不要停"，AI 也可能拍着胸脯说"全部通过"，结果你一测，bug 一堆。

ralph loop 最大的优势，就是像它的名字来源一样，"天真执着"地执行需求。只要没达到终止条件，它会一直干下去，直到任务完成，或者把你的 API 配额烧光。

最适合使用 ralph loop 的场景是：有详细的 PRD 和测试用例，但人工执行要很久。这个时候，可以让它"笨笨地坚持"、不知疲倦地按照需求干活，不用担心跑偏。

至于 token 成本，反正人工执行也要花那么多，不用太纠结。

## 安装和基础使用

安装非常简单，在 Claude Code 中输入以下命令：

```
/plugin marketplace add anthropics/claude-code
/plugin install ralph-loop@claude-plugins-official
```

注意现在是 ralph-loop，不是 ralph-wiggum。装错名字就得重来。

基础使用语法是这样的：

```
/ralph-loop:ralph-loop "你的需求描述" --max-iterations 20 --completion-promise "完成标记"
```

- `--max-iterations`：最大循环次数，防止无限跑下去
- `--completion-promise`：完成标记，AI 输出这个标记就停止

## 实战案例

原作者用它做了一个飞书文档一键排版工具。过程是这样的：

第一步，让 AI 帮忙写一个 PRD。把需求丢给 Raycast 或者其他 AI 工具，生成一份详细的产品需求文档，保存为 prd.md。

第二步，让 ralph loop 执行 MVP 开发：

```
/ralph-loop:ralph-loop "读取 prd.md，选择 2-3 个最核心的功能实现一个可运行的 MVP。要求：1) 有基本的用户界面 2) 核心功能可以操作 3) 包含基础测试 4) 确保代码可以运行。完成后输出 <promise>MVP_COMPLETE</promise>" --max-iterations 20 --completion-promise "MVP_COMPLETE"
```

9 分 31 秒后，一个能用的 MVP 就完成了。默认做了 3 个模板，图片粘贴正常渲染，标题格式、分级标题、代码块都映射好了。

第三步，迭代优化。可以让它增加更多模板、改进样式：

```
/ralph-loop:ralph-loop "读取 prd.md，多提供几个模板（先来 10 个吧），找一下微信上比较流行的模板样式。代码块用 mac terminal 风格，自动换行。所有模板不要首行缩进。完成后输出 <promise>TEMPLATE_ENRICH_COMPLETE</promise>" --max-iterations 20 --completion-promise "TEMPLATE_ENRICH_COMPLETE"
```

5 分钟搞定，10 个模板全部就位，微信粘贴完美兼容。

## 一个需要注意的坑

有个很诱人的用法是直接输入 `make it better` 这种模糊需求。

原作者试了一下，让 ralph loop 自己优化，跑了 14 轮循环。结果确实做了很多优化：暗黑模式、导入 HTML、快捷键、模板预览、模板搜索、分类筛选、字数统计、阅读时间……

但这些真的是核心需求吗？这就是典型的"屎上雕花"。

**结论很简单：ralph loop 适合执行明确的需求，不适合模糊的"优化"指令。** 有详细 PRD 就用它，想"变好一点"就算了。

## 总结

ralph loop 是一个强大的自动化工具，核心价值在于"笨笨地坚持"。给它一个清晰的终点，它会不知疲倦地跑下去。

适合场景：有详细 PRD、测试用例明确、人工耗时长的任务。

不适合场景：模糊的优化需求、方向不定的探索性任务。

用对了场景，它就是你的夜间生产力神器。用错了场景，就是 token 焚烧炉。

你用过 ralph loop 了吗？有什么有趣的用法和发现？欢迎交流。
