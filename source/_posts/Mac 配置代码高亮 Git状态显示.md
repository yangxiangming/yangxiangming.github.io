---
layout: post
title: "Mac 配置代码高亮 Git状态显示"
date: 2016-04-05 10:10:29 +0800
categories: notes
author: yangxiangming
tags: [mac, os x, git, 代码提示高亮]
---

Mac 一个为开发者量身定做的笔记本，相信你已经装好了`iTerm2`了，打开你的`iTerm2`开始我们的配置。
<!-- more -->
首先进入到Home到目录一般默认打开的都是Home，如果不是输入 cd ~ 回车即可，编辑Home目录下的 .bash_profile 编辑 vim .bash_profile 配置代码如下：

```bash
#<Git branch status>
function git_status {
  local unknown untracked stash clean ahead behind staged dirty diverged
  unknown='0;34' # blue
  untracked='0;32' # green
  stash='0;32' # green
  clean='0;32' # green
  ahead='0;33' # yellow
  behind='0;33' # yellow
  staged='0;96' # cyan
  dirty='0;31' # red
  diverged='0;31' # red

  if [[ $TERM = *256color ]]; then
    unknown='38;5;20' # dark blue
    untracked='38;5;76' # mid lime-green
    stash='38;5;76' # mid lime-green
    clean='38;5;82' # brighter green
    ahead='38;5;226' # bright yellow
    behind='38;5;142' # darker yellow-orange
    staged='38;5;214' # orangey yellow
    dirty='38;5;202' # orange
    diverged='38;5;196' # red
  fi

  branch=$(git rev-parse --abbrev-ref HEAD 2>/dev/null)
  if [[ -n "$branch" ]]; then
    if [[ "$branch" == 'HEAD' ]]; then
      branch=$(git rev-parse --short HEAD 2>/dev/null)
    fi
    git_status=$(git status 2> /dev/null)
    # If nothing changes the color, we can spot unhandled cases.
    color=$unknown
    if [[ $git_status =~ 'Untracked files' ]]; then
      color=$untracked
      branch="${branch}+"
    fi
    if git stash show &>/dev/null; then
      color=$stash
      branch="${branch}*"
    fi
    if [[ $git_status =~ 'working directory clean' ]]; then
      color=$clean
    fi
    if [[ $git_status =~ 'Your branch is ahead' ]]; then
      color=$ahead
      branch="${branch}"
    fi
    if [[ $git_status =~ 'Your branch is behind' ]]; then
      color=$behind
      branch="${branch}"
    fi
    if [[ $git_status =~ 'Changes to be committed' ]]; then
      color=$staged
    fi
    if [[ $git_status =~ 'Changed but not updated' ||
          $git_status =~ 'Changes not staged'      ||
          $git_status =~ 'Unmerged paths' ]]; then
      color=$dirty
    fi
    if [[ $git_status =~ 'Your branch'.+diverged ]]; then
      color=$diverged
      branch="${branch}!"
    fi
    printf "\033[%sm%s\033[0m" "$color" "($branch)"
  fi
  return 0
}

#<bles colorin the terminal bash shell export>
export CLICOLOR=1

#<sets up thecolor scheme for list export>
export LSCOLORS=gxfxcxdxbxegedabagacad

#<sets up theprompt color (currently a green similar to linux terminal)>
export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;36m\]\w\[\033[01;31m\]$(git_status)\[\033[0m\]\$ '

#<enables colorfor iTerm>
export TERM=xterm-256color
```

复制粘贴退出保存，然后执行一下 source .bash_profile 回车即可。然后我们在来开启代码高亮开关，编辑 vim .vimrc 开启配置项

```bash
#<代码高亮开启>
syntax on

#<文件内容行号>
set nu
```

保存退出就可以了。为了确保下次开机命令依然有效，执行如下命令：

```bash
echo "[ -r ~/.bashrc ] && source ~/.bashrc" >> .bash_profile
```
退出iTerm2重新打开就可以了，开始你的屌屌屌命令行高亮之旅吧！
