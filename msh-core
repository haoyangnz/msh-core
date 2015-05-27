#### TODO {{{
# General de-numbering
# cds should back up current vue dataset, and import new vue dataset automatically
# Globally fix pglc
# Block stdout for rrc
# Implement directory stack
# Make menu pager friendly - enhance cds to use menu - ds to go to moded ds
# Mount MAP rev with sshfs
# Isolate personal parts from common parts
# Sensible command names
# Usage statistics and automatic enhancement recommendation based on usage
# Dynamic Command Composition - Creating vw from v and w
# Archiving system
# Advanced post-command support
# Completing running task beeps terminal
# Command takes precedence over file open
# Auto clean of STDBUF
# }}}

#### Keymap {{{
# a : 
# b : 
# c : 
# d : 
# e : Explain
# f : Find
# g : 
# h : History
# i : 
# j : 
# k
# l : 
#*m : MAP
# n
# o : Open
# p : Print
# q
# r
# s : Search
# t
# u
#*v : Vue
# w 
# x
# y
# z
# rc : Source
# td : Todo
# }}}

#### ZSH {{{ 

# Color {{{
autoload colors
colors
for color in RED GREEN YELLOW BLUE MAGENTA CYAN WHITE; do
eval BR$color='$terminfo[bold]$fg[${(L)color}]'
eval PBR$color='%{$terminfo[bold]$fg[${(L)color}]%}'
eval BRBG$color='$terminfo[bold]$bg[${(L)color}]'
eval PBRBG$color='%{$terminfo[bold]$bg[${(L)color}]%}'
eval $color='$fg[${(L)color}]'
eval BG$color='$bg[${(L)color}]'
eval P$color='%{$fg[${(L)color}]%}'
(( count = $count + 1 ))
done
FINISH="[m"
PFINISH="%{$terminfo[sgr0]%}"
# }}}

# Prompt {{{  

autoload -U add-zsh-hook

#add-zsh-hook precmd o_precmd
#add-zsh-hook preexec o_preexec

o_preexec() {
}

top_prompt() {
  #e " ${MAGENTA}MSH-$$   $CYAN$(rnode $CDS \/ 1) $(rnode $CDS \/ 0)   $BLUE$MODE | $PJ   $GREEN$MAP_REV | $MAP_DATA_REV $FINISH"
  e " ${BRCYAN}MSH-$$${FINISH}"
  e " $CYAN$(rnode $CDS \/ 1) $(rnode $CDS \/ 0)   $BLUE$MODE | $PJ   $GREEN$MAP_REV | $MAP_DATA_REV $FINISH"
  e $BRCYAN-------------------------------------------------------------------------------------------------------$FINISH
  usr_line="${BLUE} USR: $MSH_INTER"
  pwd_lth=$(echo $LAST_PWD | wc -c)
  usr_line_lth=$(echo $usr_line | decolor | wc -c)
  printf  "$usr_line%*s$BRCYAN$LAST_PWD$FINISH\n" $(( $(tput cols) - $usr_line_lth - $pwd_lth + 1 ))
  e $BRCYAN-------------------------------------------------------------------------------------------------------$FINISH
  e "${MAGENTA} MSH: $MSH_REPLY$FINISH"
  e $BRCYAN-------------------------------------------------------------------------------------------------------$FINISH
}

# Window Title
wt() {
	if [[ $1 != "" ]]; then
		wtl $1
	else
		wtu
	fi
}

# Window Title Lock
# Set tmux window title and prevent further changes
wtl() {
  if [[ $1 != "" ]]; then
    wtt $1
  fi
  MSH_WT=false
}

# Window Title Unlock
# Set tmux window title and allow further changes
wtu() {
  MSH_WT=true
  if [[ $1 != "" ]]; then
    wtt $1
  fi
}

# Test whether tmux window title is allowed to change
wt_test() {
  e $MSH_WT
}

# Window Title Temporary
wtt() {
  if $(wt_test); then
    print -Pn "\033k$1\033\\"
  fi
}

# TODO Tab to expand current word - progress guess
msh-guess() {
  BUFFER=$(echo $BUFFER | head -1)
  BUFFER_CHAR=$(echo $BUFFER | wc -c)
  if [[ $BUFFER == "" ]]; then
    MSH_GUESS="Hello"
  else
    if ! evn "e \"$BUFFER\" | g \" \"" && evn "lfd $MSH_GUESS_INDEX $BUFFER*"; then
      MSH_GUESS=$(eve "lfd $MSH_GUESS_INDEX $BUFFER*")
    #elif evn "lfd $MSH_GUESS_INDEX $(rnode \"$BUFFER\" \" \" 0)*"; then
    #  MSH_GUESS=$(eve "lfd $MSH_GUESS_INDEX $(rnode \"$BUFFER\" \" \" 0)*")
    else
      MSH_GUESS=$(chd "$BUFFER" $MSH_GUESS_INDEX)
    fi
  fi
  BUFFER+="
 MSH: $MSH_GUESS ?"
  region_highlight=("$BUFFER_CHAR $#BUFFER fg=magenta");
}

msh-guess-first() {
  MSH_GUESS_INDEX=1
  msh-guess
}
self-insert() {
  zle .self-insert
  msh-guess-first
}
zle -N self-insert
backward-delete-char() {
  zle .backward-delete-char
  msh-guess-first
}
zle -N backward-delete-char
vi-backward-delete-char() {
  zle .vi-backward-delete-char
  msh-guess-first
}
zle -N vi-backward-delete-char
delete-char() {
  zle .delete-char
  msh-guess-first
}
zle -N delete-char
vi-delete-char() {
  zle .vi-delete-char
  msh-guess-first
}
zle -N vi-delete-char

msh-guess-next() {
  (( MSH_GUESS_INDEX = $MSH_GUESS_INDEX + 1 ))
  msh-guess
}
zle -N msh-guess-next
bindkey "^N" msh-guess-next

msh-guess-previous() {
  (( MSH_GUESS_INDEX = $MSH_GUESS_INDEX - 1 ))
  msh-guess
}
zle -N msh-guess-previous
bindkey "^P" msh-guess-previous

msh-personality() {
# Intepretation
  if [[ $BUFFER == "" ]]; then
    MSH_INTER="Go home"
  else
    MSH_INTER=$BUFFER
  fi
# Reply
  if [[ $BUFFER == $LAST_BUFFER ]]; then
    MSH_REPLY="You're nagging again.."
  elif [[ $BUFFER == "Hello" ]]; then
    MSH_REPLY="Hey. By the way - you're hardly this polite. Is everything OK?"
  elif [[ $LAST_BUFFER == "Hello" && $BUFFER == "No" || $BUFFER == "No" ]]; then
    MSH_REPLY="Oh.. What's wrong?"
  elif [[ $LAST_BUFFER == "No" ]]; then
    MSH_REPLY="OK. Good luck."
  elif [[ $BUFFER == "I like you" ]]; then
    MSH_REPLY="Aww.. How sweet of you :)"
  elif [[ $BUFFER == "I like"* ]]; then
    MSH_REPLY="I like it too"
  elif [[ $BUFFER == *"Haoyang"* ]]; then
    MSH_REPLY="I think Haoyang is a genius."
  else
    MSH_REPLY="OK"
  fi
}

msh-enter() {
  wtt $(lnode $BUFFER " " 1)
  msh-personality
  LAST_BUFFER=$BUFFER
  LAST_PWD=$(pwd)
  cs
  zle expand-word
  if [[ $BUFFER == "" ]]; then
    BUFFER="d"
  elif [[ -a $BUFFER || $BUFFER = "-" ]]; then 
    BUFFER="o $BUFFER"
  else
    BUFFER=$LAST_BUFFER
    if which $(lnode $BUFFER " " 1) &> /dev/null; then
      e $BUFFER >> $MSH_HISTCMD
    fi
  fi
  zle accept-line
}

msh-accept-guess() {
# Accept guess 
  BUFFER=$MSH_GUESS
  msh-enter
}
zle -N msh-accept-guess
bindkey "^@" msh-accept-guess

msh-new-command() {
# Remove guess
  BUFFER=$(echo $BUFFER | head -1)
  msh-enter
}
zle -N msh-new-command
bindkey "\r" msh-new-command

msh-expand() {
  BUFFER=$(tn $BUFFER)
}
zle -N msh-expand
bindkey "^E" msh-expand

msh-history() {
  BUFFER=$(echo $BUFFER | head -1)
  BUFFER="ch $BUFFER"
  msh-enter
}
zle -N msh-history
bindkey "^H" msh-history

msh-forward() {
  BUFFER=n
  msh-enter
}
zle -N msh-forward
bindkey "^F" msh-forward

msh-backward() {
  BUFFER=p
  msh-enter
}
zle -N msh-backward
bindkey "^B" msh-backward

PROMPT="
${PBRCYAN}-------------------------------------------------------------------------------------------------------${PFINISH}
${PBLUE} USR: ${PFINISH}"
RPROMPT="${PBRCYAN}%/${PFINISH}"

# }}}

# Editor {{{
export EDITOR=vim
bindkey -M vicmd '?' history-incremental-search-backward
# }}}

# Syntax {{{
#setopt extended_glob
#TOKENS_FOLLOWED_BY_COMMANDS=('|' '||' ';' '&' '&&' 'sudo' 'do' 'time' 'strace')
# 
#recolor-cmd() {
#   region_highlight=()
#   colorize=true
#   start_pos=0
#   for arg in ${(z)BUFFER}; do
#       ((start_pos+=${#BUFFER[$start_pos+1,-1]}-${#${BUFFER[$start_pos+1,-1]## #}}))
#       ((end_pos=$start_pos+${#arg}))
#       if $colorize; then
#           colorize=false
#           res=$(LC_ALL=C builtin type $arg 2>/dev/null)
#           case $res in
#               *'reserved word'*)   style="fg=magenta";;
#               *'alias for'*)       style="fg=cyan";;
#               *'shell builtin'*)   style="fg=yellow";;
#               *'shell function'*)  style='fg=green';;
#               *"$arg is"*)
#                   [[ $arg = 'sudo' ]] && style="fg=red" || style="fg=blue";;
#               *)                   style='none';;
#           esac
#           region_highlight+=("$start_pos $end_pos $style")
#       fi
#       [[ ${${TOKENS_FOLLOWED_BY_COMMANDS[(r)${arg//|/\|}]}:+yes} = 'yes' ]] && colorize=true
#       start_pos=$end_pos
#   done
#}
#
#
#backward-delete-char() { zle .backward-delete-char && recolor-cmd }
# 
#zle -N backward-delete-char
# }}}

# History {{{
export HISTSIZE=10000
export SAVEHIST=10000
export HISTFILE=~/.zhistory
setopt INC_APPEND_HISTORY
setopt HIST_IGNORE_DUPS
setopt EXTENDED_HISTORY     
setopt AUTO_PUSHD
setopt PUSHD_IGNORE_DUPS
setopt HIST_IGNORE_SPACE
# }}}

# Options {{{
#setopt NULL_GLOB
# }}}

# Development Environment {{{
#PATH=$PATH:$HOME/.rvm/bin:~/bin # Add RVM to PATH for scripting
#source ~/.rvm/scripts/rvm

# }}}

# Completion {{{

#. $MOS_ROOT/src/zsh-autosuggestions/autosuggestions.zsh
#zle-line-init() {
#  zle autosuggest-start
#}
#zle -N zle-line-init
#bindkey '^T' autosuggest-toggle


#autoload predict-on
#predict-toggle() {
#    ((predict_on=1-predict_on)) && predict-on || predict-off
#}
#zle -N predict-toggle
#bindkey '^T' predict-toggle
#zstyle ':predict' toggle true
#zstyle ':predict' verbose true

# }}}

#}}}

#### MSH {{{

MSH_LS_AUTO_MODE=false
MSH_AUTO_EXCLUDE=true
MSH_AUTO_EXCLUDE_GLIST=".svn,.git,testsrc,target,.classpath,node_modules,bower_components,lib"
MSH_AUTO_EXCLUDE_FLIST="! -path '*/.svn/*' ! -path '*/target/*' ! -path '*/testsrc/*' ! -path '*/classes/*'"
MSH_AUTO_EXCLUDE_TLIST=".svn|testsrc|target|.classpath|node_modules|.git|lib|bower_components"

# }}}

#### General OS {{{

# Core Environment variables {{{

# OS can be GNU or BSD
export OS=GNU

# TODO Move personal stuff to prc
export PASS=TODO
export PRINTER=Canon_LBP6780_3580_UFR_II
export BEEP=/usr/share/sounds/ubuntu/stereo/message-new-instant.ogg
export LANG=en_NZ.UTF-8

export MOS_ROOT=~/mos

export WORK=$MOS_ROOT/work
export COMMON=$MOS_ROOT/common
export SANDBOX=$MOS_ROOT/sandbox
export NOTE=$MOS_ROOT/note
export MOS_BIN=$MOS_ROOT/bin
export MOS_LIB=$MOS_ROOT/lib
export MNT=$MOS_ROOT/mnt

export MOS_TMP=$WORK/.tmp
export STDOUT=$MOS_TMP/stdout/$$
export STDERR=$MOS_TMP/stderr/$$
export STDBUF=$MOS_TMP/stdbuf/$$
export MSH_HISTCMD=$MOS_TMP/HISTCMD
export MSH_HISTOPEN=$MOS_TMP/HISTOPEN
export MSH_HISTOPENFILE=$MOS_TMP/HISTOPENFILE

[[ $OS == "BSD" ]] && export CLICOLOR_FORCE=1

# }}}

# OS Specific environment variables {{{

[[ $OS == "BSD" ]] && export HOMEBREW_GITHUB_API_TOKEN="e8f1e7cc39091e3114c790cfe2fd7736bdbbebf2"

# }}}

# Core Aliases {{{

alias ja='mvn clean install'
alias jats='mvn clean install -Dmaven.test.skip'
alias pe='mvn eclipse:eclipse'
alias dp='deploy'
alias jr='jrebel'
alias pls='sudo $(!!)'
alias dp='deploy'

alias beep='paplay $BEEP'

alias bc='bc -l'
alias emacs='emacs -nw'

case $OS in
  GNU) alias lscolor='ls --color'
	     alias lsnocolor='ls --color=none';;
	BSD) alias lscolor='gls --color'
	     alias lsnocolor='gls --color=none';;
esac

# }}}

# Utility functions {{{


# Left Node
# lnode a/b/c/d / 2 : Split a/b/c/d by delimiter / to [a,b,c,d] and obtain the second substring b
lnode() {
  echo $1 | cut -d $2 -f $3
}

# Right Node
# rnode a/b/c/d / 2 : Split a/b/c/d by delimiter / to [a,b,c,d] and obtain the second to last substring b (0 based)
rnode() {
  echo $1 | cut -d $2 -f $(echo "$(echo $1 | g -o $2 | wc -l) + 1 - $3" | bc)
}

# Print previous command
pc() {
  fc -nl 1 | gv "^[ef]?[0-9]*$" | tail -1
}

# Run previous command
rpc() {
  eval $(pc)
}

# Produce a numbered menu from the ouput of a command and return the selected line
# $@ the command to execute
menu() {
  eval $@ | nl
  read n
  menu=$(eval $@ | sed -n "$n"p | decolor)
}

# Text Manipulation: Keyword
# kw one two three : Outputs "one.*two.*three"
kw() {
  echo $@ | s "s/ /.*/g"
}

# Text Manipulation: Full PascalCase Abbreviation
# Form a regex for full PascalCase abbreviation
ab() {
  var=`echo $1 | sed 's/./&[^A-Z]*/g'`
  echo $var
}

# Form a regex for partial abbreviation
pab() {
  var=`echo $1 | sed 's/./&.*/g'`
  echo $var
}

# Escape for sed replacement
esr() {
 sed -e 's/[\/&]/\\&/g'
}

# Escape for regex
esk() {
  sed -e 's/[]\/(){}$*.^|[]/\\&/g'
}

# Start second-level cron job
# TODO Make friendly with pager
scron() {
  if [[ $2 = "" ]]; then
    SLEEP_TIME=1;
  else
    SLEEP_TIME=$2
  fi
  while true; do
    evb $1
    clear
    catb
    sleep $SLEEP_TIME;
  done 
}

# Start second-level cron daemon
# TODO Make friendly with pager
scrond() {
  if [[ $2 = "" ]]; then
    SLEEP_TIME=1;
  else
    SLEEP_TIME=$2
  fi
  while true; do
    evn $1
    sleep $SLEEP_TIME;
  done &
}

# Execute a command with the arguments from a file and open the output in vi
# $1 command to execute
# $2 file that contains all the arguments
# exe sqle queries : Execute sqle with every line in the file "queries" as argument
exe() {
  while read args; do
    echo $1 $args
    $1 $args
    echo
  done < $2
}

# Remove line numbers
unl() {
  cut -f2-
}

# Date : Timestamp
# timestamp : Show current time stamp in nano second
timestamp() {
  date +%s%N
}

# }}}

# Pager {{{

# Pager : Pager Size
# PAGER_SIZE=30 : Set the pager size to 30
PAGER_SIZE=15

# Pager : Pager Print
# pgp : Print the current page
pgp() {
  cs && echo
  e $MAGENTA $(($PAGER_TOP / $PAGER_SIZE + 1))/$(($(catb | wc -l) / $PAGER_SIZE + 1)) $FINISH${BLUE}Selected:$SI$FINISH
  catb | cut -c -170 | nl | al $PAGER_TOP $PAGER_BOTTOM
}

# Pager : Pager Line Count
# pglc : Print the total number of lines in the pager buffer
pglc() {
  catb | wc -l
}

# Pager : Pager
# pg : Print the first page
pg() {
  PAGER_TOP=1
  PAGER_BOTTOM=$PAGER_SIZE
  pgp
}

# Pager : Pager Next
# n : Print the next page
n() {
  (( PAGER_TOP += $PAGER_SIZE ))
  (( PAGER_BOTTOM += $PAGER_SIZE ))
  pgp
}

# Pager : Pager Previous
# p : Print the previous page
p() {
  (( PAGER_TOP -= $PAGER_SIZE ))
  (( PAGER_BOTTOM -= $PAGER_SIZE ))
  pgp
}

# }}}

# System functions {{{

# Verbal Test
# vt "[[ -a src ]]" : Test if src exists, and print the result
vt() {
  eval $1 && e \"$1\" is true || e \"$1\" is false
}

# Clear Screen
cs() {
  clear
  top_prompt
}

msh-clear() {
  BUFFER=cs
  msh-enter
}
zle -N msh-clear
bindkey "^L" msh-clear


clip() {
  xclip -sel clip
}

decolor() {
  sed "s/\[\([0-9]\{1,2\}\(;[0-9]\{1,2\}\)\{0,1\}\)\{0,1\}[m|K]//g" $@
}

# Evaluate : Evaluate to Buffer
# evb : evaluate
evb() {
  eval $@ &> $MOS_TMP/stdbuf/$$
  decolor $MOS_TMP/stdbuf/$$ > $MOS_TMP/stdbuf/$$.nocolor
}

catb() {
  cat $MOS_TMP/stdbuf/$$
}

catbnc() {
  cat $MOS_TMP/stdbuf/$$.nocolor
}

catlb() {
  catl $MOS_TMP/stdbuf/$$ $1
}

catlbnc() {
  catl $MOS_TMP/stdbuf/$$.nocolor $1
}

cato() {
  c $STDOUT
}

cate() {
  c $STDERR
}

evn() {
  eval $@ &> /dev/null
}

w() {
  which $@
}

wn() {
  evn w $@
}

pi() {
  e --- Installing $@
	case $OS in
			GNU) sudo apt-get install $@;;
			BSD) brew install $@;;
	esac
}

pr() {
  e --- Uninstalling $@
  sudo apt-get remove $@
}

# }}}

# File {{{

# Cat
c() {
  if [[ -f $1 ]]; then
    cat $1;
  fi
}

cr() {
	case $OS in
			GNU) tac $@;;
			BSD) tail -r $@;;
	esac
}

# Tar

z() {
	if [[ $1 == "" ]]; then
		tar cvfz $(rnode $(pwd) / 0).tar.gz *
	elif [[ $2 == "" ]]; then
		tar cvfz $1.tar.gz $1
	else
		tar cvfz $1.tar.gz ${@:2}
  fi
}

zr() {
	z $@
	if [[ $1 == "" ]]; then
		r *
	elif [[ $2 == "" ]]; then
		r $1
	else
		r ${@:2}
  fi
}

uz() {
  for target in $@; do
    echo $target
    if [[ $target = *.tar.gz ]]; then
      tar xvfz $target
      r $target
    elif [[ $target = *.gz ]]; then
      gunzip $target
      r $target
    elif [[ $target = *.tar ]]; then
      tar xvf $target
      r $target
    elif [[ $target == *.bz2 ]]; then
      bunzip2 $target
      r $target
    fi
  done
}

# Back Up : Back Up
bu() {
  if [[ $2 = "" ]]; then
    cp -r $1 $1.bak
  else
    cp -r $1 $1.bak.$2
  fi
  l
}

# Back Up : Back Up Remove
bur() {
  mv $1 $1.bak
}

# TODO pbr
# Back Up : Put Back
pb() {
  r $1
  if [[ $2 == "" ]]; then
    cp -r $1.bak $1
  else
    cp -r $1.bak.$2 $1
  fi
}

pbr() {
  r $1
  m $1.bak $1
}

# TODO
# Directory history
#d() {
#  pn d "dirs -v | head -10"
#}

# Tree
t() {
  if $MSH_AUTO_EXCLUDE; then
    pn t "tree -fC --noreport -I \"$MSH_AUTO_EXCLUDE_TLIST\" $@"
  else
    pn t "tree -fC --noreport $@"
  fi
}

tl() {
  pn t "tree -f --noreport -L $1 ${@:2}"
}

for ((i = 0; i < 100; i++)); do
  alias t$i="tl $i"
done

# Tree diff
tdiff() {
  d $1
  tree > $MOS_TMP/tdiff.1
  d -
  d $2
  tree > $MOS_TMP/tdiff.2
  d -
  vimdiff $MOS_TMP/tdiff.1 $MOS_TMP/tdiff.2
  rm $MOS_TMP/tdiff.1
  rm $MOS_TMP/tdiff.2
}

function command_not_found_handler() {
  cs
}

# List Utility : List
# l : List files in full or brief depending on total number of files
l() {
  if $MSH_LS_AUTO_MODE && [[ $(la $@ | wc -l) -lt 30 ]]; then
    la $@
  else
    lb $@
 fi
}

# List Utility: List All
# la : List almost all files
la() {
  pn l "lscolor -ltuhA $@ | gv ^total"
	wtt $(rnode $(pwd) "/" 0)
}

# List Utility: List Brief
# lb : List files in brief
lb() {
  pn l "lscolor -ltuh $@ | gv ^total"
  wtt $(rnode $(pwd) "/" 0)
}

# List Utility: List Hidden
# lh : List hidden files
lh() {
  pn l "lscolor -ltuhd .*"
  wtt $(rnode $(pwd) "/" 0)
}

# List : List Filtered
# lf : List files with filename pattern filter
lf() {
  pn l "lscolor -ltuhd $@"
  wtt $(rnode $(pwd) "/" 0)
}

lfd() {
  lsnocolor -tuhd ${@:2} | head -$1 | tail -1
}

# Shortcut : Shortcut
# sc a/b : Create shortcut b pointing to a/b (shortcut name is taken from leaf of destination)
# sc a/b c : Create shortcut c pointing to a/b
sc() {
  ln -fs $@
}


# Move : Move
# m a b c : Move a and b to c
m() {
  mv ${@:1:-1} $@[-1]
  l
}

# Move : Move Shortcut
# ml a b c : Move a and b to c, and create symbolic links from a and b to c/a and c/b
ms() {
  m $@
  for file in ${@:1:-1}; do
    sc $@[-1]/$file
  done
  l
}

export TRASH=$MOS_ROOT/.Trash
export ARCHIVE=$MOS_ROOT/.Archive

# Remove Utility: Remove
# r file : Move file to trash
r() {
  trash=$TRASH/$(timestamp)
  md $trash
  if [[ $1 == "" ]]; then
    mv * $trash
  elif [[ -a $1 ]]; then
    mv $@ $trash
  fi
  l
}

# Remove Utility: Remove Undo TODO Create history aware Undo TODO Doesn't work when removed nested file
# ru : Undo the last removal
ru() {
  m $TRASH/$(ls $TRASH | tail -1)/*(D) .
	rmdir $TRASH/$(ls $TRASH | tail -1)
}

# Remove Utility: Remove List
# rl : List files in trash
rl() {
  l $TRASH
}

# Remove Utility: Remove Empty
# re : Empty files in trash
re() {
  rm -rf $TRASH/*
}

# Remove : Archive
# ra a : Archive file a
ra() {
  archive=$ARCHIVE/$(timestamp)
  md $archive
  if [[ $1 == "" ]]; then
    mv * $archive
  elif [[ -a $1 ]]; then
    mv $@ $archive
  fi
  l
}

# Cat Utility: Cat Line
# catl file 1 : Cat file line 1
catl() {
  sed -n $2p $1
}

# Write Utility: Insert Line
# ins file "text" : Insert "text" into line 1 of file
ins() {
  sed -i "1i$2" $1
}

# }}}

# Configuration Files {{{

export RC=$MOS_ROOT/.rc
export ZSHRC=$RC/.zshrc
export VIMRC=$RC/.vimrc
export TMUXRC=$RC/.tmux.conf
export MUTTRC=$RC/.muttrc
export XMODMAP=$RC/.Xmodmap
export DIRCOLORSRC=$RC/.dir_colors

sc $ZSHRC ~
sc $VIMRC ~
sc $TMUXRC ~
sc $MUTTRC ~
sc $DIRCOLORSRC ~

# Re-source Zsh
rrc() {
  source $ZSHRC
}

# Configure Zsh
rc() {
  echo ":set foldmethod=marker" > $MOS_TMP/rc.vi
  echo "zR/^$1\(" > $MOS_TMP/rc1.vi
  if [[ $1 = "" ]]; then
    vi $ZSHRC -s "$MOS_TMP/rc.vi"
  else
    vi $ZSHRC -s "$MOS_TMP/rc1.vi"
  fi
  source $ZSHRC
}

# Regression Test RC
rtrc() {
# Prepare work directory
  pwd=$(pwd)
  sb
  d rt
  cs
# Perform regression test
  result=PASSED
  e --- Starting RT
  if [[ $(rrc) != "" ]]; then
    e rrc has output
    result=FAILED
  fi
# Remove work directory
  cd $pwd
  if [[ $result == "FAILED" ]]; then
    e --- RT FAILED
    return 1
  fi
  e --- RT PASSED
}

# Commit and push configuration files
circ() {
  if ! rtrc; then
    e Commit ommitted due to RT failure.
    return
  fi
  pushd .
  cd $RC
  ci $1
  git push origin master
  popd
}

# Commit and push configuration files
ciarc() {
  pushd .
  cd $RC
  cia $1
  git push -f origin master
  popd
}

# Help: Keyword function help
# h func : List all function documentation with the keyword func
h() {
  pattern="#.*$(kw $@)"
  cat $ZSHRC | gv "\(\)" | gv "^$" | gv "\{\{\{" | g -C1 "$pattern"
}

# Help: Full precise function help
# fh func : Show the documentation and code of function func
fh() {
  cat $ZSHRC | gc "^$1\(" -B2 | head -2
  w $1
}

# TODO seem to erase MAP_REV_SOURCE to MAP_REV
# Change variable in Zsh source
crc() {
  rep=$(echo $2 | esr)
  sed -i -r 's/^export '$1'=.*$/export '$1'='$rep'/gi' $ZSHRC
  rrc
}

# Configure Vi
vrc() {
  o $VIMRC
}

# Configure Tmux
trc() {
  vi $TMUXRC
  tmux source-file $TMUXRC
}

# Configure Mutt
mrc() {
  vi $MUTTRC
}

# Re-source Xmodmap
keyon() {
  xmodmap $XMODMAP
}

# }}}

# Terminal {{{

alias tmo='tmux new -s'
alias tmn='tmux new -ds'
alias tmk='tmux kill-session -t'
alias tma='tmux attach -t'
alias tmr='tmux attach -d -t'
alias tms='tmux switch -t'
alias tml='tmux ls'
alias tmd='tmux detach'
alias tmw='tmux neww -t'
alias tmg='tmux selectw -t'
alias tmt='tmux send -t'

b() {
 clear
 figlet -f big -w 160 $@
}

std() {

  rrc

  if [[ $1 = "" ]]; then
    tmux kill-server
  fi

  BASE="$HOME"
  cd $BASE

  if [[ $1 = "" || $1 = "1" ]]; then
    tmk Personal
    tmn Personal
		tmw Personal:2
    tmt Personal:1 "note" C-m
    tmt Personal:2 "work" C-m
    tmg Personal:1
  fi

  if [[ $1 = "" || $1 = "2" ]]; then
    tmk Browsing
    tmn Browsing
    tmg Browsing:1
  fi

  if [[ $1 = "" || $1 = "3" ]]; then
    tmk Primary
    tmn Primary
    tmt Primary:1 "pj" C-m
    tmg Primary:1
  fi

  if [[ $1 = "" || $1 = "4" ]]; then
    tmk Secondary
    tmn Secondary -n Secondary
  fi

  tma Primary
}

# }}}

# {{{ Irssi

dh() {
  dehilight
}

clog() {
  if [[ $1 = "" ]]; then
    #pn "for file in $(find ~/.irclogs -type f); do ls -l $file; done | grep $(date +%F) | cut -d \" \" -f 8 | grep \"\/[^\/]*@\" | nl"
    pn clog "for file in $\(ls\); do echo $file; done"
  else
    o $(for file in $(find ~/.irclogs -type f); do ls -l $file; done | g $(date +%F) | g $1 | cut -d " " -f 8 | g "\/[^\/]*@")
  fi
}

# }}}

# Echo {{{

# Echo
# e text : Echo "text"
e() {
  echo $@
}

ev() {
  eve "e \$$1:u"
}

# Eval Echo
# eve {1..3}' r' : Evaluate "1 r" "2 r" "3 r"
eve() {
  eval $(e $@)
}

efp() {
  echo $PWD/$1
}

# }}}

# Awk {{{

# Awk : Awk Line
# al 1 10 : Print from line 1 to 10
al() {
  SNA="NR < 0"
  for sn in $SN; do
    SNA+=" || NR == $sn"
  done
  awk "NR >= $1 && NR <= $2 && ( $SNA ) {printf selected}
       NR >= $1 && NR <= $2 && NR % 2 == 0 {print altbg \$0 finish}
       NR >= $1 && NR <= $2 && NR % 2 == 1 {print \$0}" \
       selected=$BLUE altbg=$BGWHITE finish=$FINISH
}

# Process : Process Line
# pl 2 10 : Print from line 2 to 10
# pl 2 : Print from line 2 to the end
pl() {
  if [[ $2 == "" ]]; then
    awk "NR >= $1 {print \$0}"
  else
    awk "NR >= $1 && NR <= $2 {print \$0}"
  fi
}

# }}}

# Sed {{{

# Sed Utility: Sed
# s : Sed
s() {
  sed $@
}

# Sed Utility: inline
# si : Sed inline
si() {
  sed -i $@
}

# }}}

# Grep {{{

g() {
  grep -Ei --line-buffered --color=auto $@
}

gc() {
  grep -Ei --line-buffered --color=always $@
}

# Grep Utility: Reverse grep
# gv a : Reverse grep "a"
gv() {
  g -v $@
}

ge() {
	gr exception
}

gr() {
  if [[ $2 = "" ]]; then
    if $MSH_AUTO_EXCLUDE; then
      pn g "grep --color=always -EirnI --exclude-dir={$MSH_AUTO_EXCLUDE_GLIST} \"$1\" ."
    else
      pn g "grep --color=always -EirnI \"$1\" ."
    fi
  else
    if $MSH_AUTO_EXCLUDE; then
      pn g "grep --color=always -EirnI --include=\"$2\" --exclude-dir={$MSH_AUTO_EXCLUDE_GLIST} \"$1\" ."
    else
      pn g "grep --color=always -EirnI --include=\"$2\" \"$1\" ."
    fi
  fi
}

gs() {
  if [[ $2 = "" ]]; then
    pn gs "grep --color=always -EirnI --exclude-dir={.svn,testsrc,target,.classpath} --exclude=\"*.sql\" \"$1\" ."
  else
    pn gs "grep --color=always -EirnI --exclude-dir={.svn,testsrc,target,.classpath} --include=\"*$2*\" \"$1\" ."
  fi
}

gsa() {
  if [[ $2 = "" ]]; then
    eval $(echo "grep --color-always -EirnI --exclude-dir={.svn,testsrc,target,.classpath,inf} \"$1\" .") | nl
  else
    eval $(echo "grep --color-always -EirnI --exclude-dir={.svn,testsrc,target,.classpath,inf} --include=\"*$2*\" \"$1\" .") | nl
  fi
}

gsp() {
  eval $(echo "grep --color-always -EirnI --exclude-dir={.svn,testsrc,inf} $@ .")
}

gf() {
  pn g "grep \"^[^C].*($1)\" -rin . --include=\"*.f\" -B9999 | grep \"\s\sprogram\s|\s\sfunction\s|\s\ssubroutine\s|^[^C].*($1)\" -i | grep \"^[^C].*($1)\" -B1 -i | grep -v \"\-\-\""
}

gjf() {
  gs "(private|protected|public)[^=]*$1[^\(]*;"
}

WHC=$MOS_TMP/wh/current
WHH=$MOS_TMP/wh/history

wh() {
  if [[ ! -d $WHH ]]; then
    mkdir -p $WHH
  fi
  if [[ -f $WHC ]]; then
    mv $WHC $WHH/$(ls -1 $WHH | wc -l)
  fi
  $(whs $1) > WHC
  cat $MOS_TMP/wh/current | nl
}

whs() {
  gs "private ($1|List<$1>) " | sed "s/^.*\///" | sed "s/\..*List.*$/ */" | cut -d "." -f 1
}

# }}}

# Find {{{

# Find : Find Locate
# fl intellij : Find the system file with the keyword "intellij" in its name
fl() {
  pn fl "locate -bi $1 | gv .Trash | gv .cache | g $1"
}

# Find by full file name
f() {
  if $MSH_AUTO_EXCLUDE; then
    pn f "find $2 -regex '.*/$1' $MSH_AUTO_EXCLUDE_FLIST ${@:3}"
  else
    pn f "find $2 -regex '.*/$1' ${@:3}"
  fi
}

# Find by partial name
fp() {
  f ".*"$1"[^\/]*" ${@:2}
}

# Find source by full PascalCase abbreviated name
fa() {
  fs $(ab $1)
}

# Find source by partial PascalCase abbreviated name
fpa() {
  fps $(pab $1)
}

# }}}

# Process {{{

# Process : Process Find
# pf java tss : List all processes that includes "java" and "tss" in its details, order significant
pf() {
  ps -ef | gv grep | g $(kw $@)
}

# Process : Process Kill
# pk java tss : Kill all processes that includes "java" and "tss" in its details, order significant
pk() {
	case $OS in
			GNU) pf $@ | cut -d " " -f 2 | xargs kill -9;;
			BSD) pf $@ | cut -d " " -f 4 | xargs kill -9;;
	esac
}

# }}}

# Remote {{{

# Remote : Remote Copy
# rmc a:/b/ c:/d : Remotely copy from a:/b to c:/d
# rmc a:/b/ c:/d *.zip: Remotely copy *.zip from a:/b to c:/d
rmc() {
  if [[ $3 != "" ]]; then
    rsync -rLpt $1 $2 --include=$3 --exclude="*"
  else
    rsync -rLpt $1 $2
  fi
}

# Remote : Remote Update
# rmu a:/b/ c:/d : Remotely update c:/d to match a:/b
# rmu a:/b/ c:/d *.zip: Remotely update c:/d to match a:/b/*.zip
rmu() {
	if [[ $2 == "" ]]; then
    rsync -rLpt --delete-excluded $1 .
  elif [[ $3 == "" ]]; then
    rsync -rLpt --delete-excluded $1 $2
  else
    rsync -rLpt --delete-excluded $1 $2 --include="$3" --exclude="*"
  fi
}

# Remote : Remote Mount
# rmm a:/b/ c:/d : Remotely mount a:/b/ to c:/d
rmm() {
  md $2
  sshfs $1 $2
}

# Remote : Remote Unmount
# rmum a:/b/ : Remotely unmount a:/b/
rmum() {
  fusermount -u $1
  r $1
}

# }}}

# Directory/Disk {{{

# Directory : Make Directory
md() {
  mkdir -p $@
}

# Directory : Directory
# d ex : Move to directory "ex" even if it doesn't exist
d() {
  if [[ $1 == "" ]]; then
    1=$MOS_ROOT
  fi
  if [[ ! -d $1 && $1 != "-" ]]; then
    mkdir -p $@
  fi
  touch -a $1
  builtin cd $1
  l
}

# Disk : Usage
# du : Show a break down of the disk usages for the current directory
du() {
  #/usr/bin/du -h --max-depth=1

  # Double du
  #/usr/bin/du --max-depth=1 | sort -nr | cut -f2 | xargs -d '\n' du -sh

  # Schwartzian transform
  /usr/bin/du -h --max-depth=1 | perl -e '%byte_order = ( G => -3, M => -2, K => -1, k => -1 ); print map { $_->[0] } sort { $byte_order{$a->[1]} <=> $byte_order{$b->[1]} || $b->[2] <=> $a->[2] } map { [[ $_, /([MGK])/, /(\d+)/ ]] } <>' | head -30
}

# Disk : Free
# df : Show free disk space
df() {
  /bin/df -h
}



# }}}

# Open {{{

# Open : Open File Type (Variable)
# OPT=(png eog) : "o a.png" executes "eog a.png"
typeset -A OFT
OFT=(png eog jpg eog pdf evince zip uz gz uz jar oj)

# Open : Open
# o a : Go to into directory or open file in Vi
o() {
  if [[ $1 = "" ]]; then
    oh
    return
  fi
  e $(efp $1) >> $MSH_HISTOPEN
  if [[ -d $1 || $1 = "-" ]]; then
    d $1
  elif [[ -f $1 ]]; then
    wtt $(rnode $1 "/" 0)
    e $(efp $1) >> $MSH_HISTOPENFILE
    if [[ -x $1 ]]; then
      $1
    else
      for ft in ${(k)OFT}; do
        if [[ $1 = *.$ft ]]; then
          $OFT[$ft] $1
          return
        fi
      done
      if [[ $2 = "" ]]; then
        v $1
      else
        v $1 $2
      fi
    fi
  fi
}

oj() {
  java -jar $1
}

# History Utility : Open History File
# ohf histfile [keyword] : Show open history for a specific history file
ohf() {
  if [[ $2 == "" ]]; then
    pn oh "cr $1 | uniq -c"
    #| sort | uniq -c | sort -nr"
  else
    pn oh "cr $1"
  fi
}

# History : Open History
# ofh : Show open history
oh() {
  ohf $MSH_HISTOPEN $1
}

# History : Open File History
# ofh : Show open file history
ofh() {
  ohf $MSH_HISTOPENFILE $1
}

# History : Command History
# ch [xml] : Show command history [that involves xml]
ch() {
  if [[ $1 == "" ]]; then
    pn ch "cr $MSH_HISTCMD | gv \"^[^ ]*$\" | gv \"^ch$\" | gv \"^ch \" | uniq -c"
  else
    pn ch "cr $MSH_HISTCMD | gv \"^[^ ]*$\" | gv \"^ch$\" | gv \"^ch \" | g "$(echo $1 | esk)" | uniq -c"
  fi
}

chd() {
  cr $MSH_HISTCMD | gv "^[^ ]*$" | gv "^ch$" | gv "^ch " | g "$(echo $1 | esk)" | uniq | head -$2 | tail -1
}

# History : History Clear
# hc : Clear all history
hc() {
  : > $MSH_HISTCMD
  : > $MSH_HISTOPEN
  : > $MSH_HISTOPENFILE
}

#}}}

# Vi {{{

# Vi
v() {
  if [[ $2 = "" ]]; then
    vi $1
    pg
  else
    vi +$2 $1
    pg
  fi
}

# Find by full name and open with Vi
vf() {
  f $1
  rn 1
}

# Find by partial name and open with Vi
vp() {
  vf $1
  rn 1
}

# Find source by full name and open with Vi
vs() {
  vi $(fs $1 | unl)
}

# Find source by partial name and open with Vi
vps() {
  vi $(fps $1 | unl)
}

# Find source by full PascalCase abbreviated name and open with Vi
va() {
  vi $(fa $1 | awk '{print $2}')
}

# Find source by partial PascalCase abbreviated name and open with Vi
vpa() {
  vi $(fpa $1 | awk '{print $2}')
}

# Find executable in PATH and open with Vi
vw() {
  vi $(w $1)
}

# Vim: Vim Diff
# vd a b : Vim-diff file a and file b
vd() {
  vimdiff $@
}

# }}}

# Numbered Shortcut {{{

# Numbered Shortcut : Selected Numbers Initialise
# sni : Reset the selected numbers
sni() {
   SI=""
   SN=()
}
sni

# Numbered Shortcut : Add Number
# an 12 : Add number 12 into the number list
an() {
  for var in $@; do
    SN+=$var
    n=$(catlbnc $var)
    case $(pnc) in
        l) SI="$SI $(pwd)/$(echo $n | awk '{print $9}')";;
        *) echo Done nothing.;;
    esac
  done
  pgp
}

# Numbered Shortcut : Clear Numbers
# cn : Remove all selected numbers
cn() {
  sni
  pgp
}

# Numbered Shortcut : Selected Numbers
# sn m a : Move all selected items to a (translates to "m $(e $SI) a")
sn() {
  $1 $(e $SI) ${@:2}
  sni
}

pn() {
  echo $1 > $MOS_TMP/stdbuf/$$.cmd
  evb ${@:2}
  pg
}

pnc() {
  c $MOS_TMP/stdbuf/$$.cmd
}

fn() {
  catlbnc $1 | xargs ${@:2}
}

# Translate Numbered Shortcut
tn() {
    n=$(catlbnc $1)
    case $(pnc) in
        d) e cd +$1;;
        t) case $2 in
               r) e r $(rnode $n " " 0);;
               "") e o $(rnode $n " " 0);;
					 esac;;
        note) e o $(rpc | sed -n "$1"p | cut -d "-" -f 3 | cut -d " " -f 2);;
        l) case $2 in 
               r) e r "$(echo $n | awk '{print $9}')";;
               "") e o "$(echo $n | awk '{print $9}')";;
               *) e $2 "$(echo $n | awk '{print $9}')" ${@:3};;
           esac;;
        lt) e o $(echo $n | awk '{print $9}');;
        fl) e o $n;;
        f) e o $n;;
        fa) e o $n;;
        fp) e o $n;;
        fps) e o $n;;
        fpa) e o $n;;
        tca) e vimdiff $n $PJ_ROOT/$MB/$n;;
        oh) e o $(rnode $n " " 0);;
        ch) e $(e $n | cut -c 9-);;
        g) e o $(echo $n | cut -d ":" -f 1) $(echo $n | cut -d ":" -f 2);;
        gs) e o $(echo $n | cut -d ":" -f 1) $(echo $n | cut -d ":" -f 2);;
        gf) if echo $n | grep -q "^[^ ]*:"; then delim=:; else delim=-; fi; o $(echo $n | cut -d $delim -f 1) $(echo $n | cut -d $delim -f 2);;
        wh) e wh $(echo $n | cut -d " " -f 1);;
        st) e dt $(e $n | cut -c9-);;
        clog) e o $n;;
        jdb) e "si '/debugport/d' $n; si '/rmidport/a debugport=\"13066\"' $n";;
				br) case $2 in 
								"") if [[ $n == *remotes*origin* ]]; then e brt $(echo $n | s 's/remotes.origin.//g'); else e sw $n; fi;;
								d) e brd $(e $n | s 's/\*//g');;
								r) e brd $(e $n | s 's/\*//g');;
						esac;;
        *) echo Done nothing.;;
    esac
}

# Numbered shortcut: Run numbered shortcut # rn 2: Run the numbered shortcut for line 2 of the output of the previous command
rn() {
# Listing numbers to add to selected list quickly
  if [[ $2 != "" && $2 = [0-9]* ]]; then
    cmd=()
    for arg in $@; do
      if [[ $arg = [0-9]* ]]; then
        an $arg
      else
        cmd+=$arg
      fi
    done
    if [[ $cmd != "" ]]; then
      sn $cmd
      unset cmd
    else
      pgp
    fi
  else
    eve $(tn $@)
  fi
}

for ((i = 0; i < 10000; i++)); do
  alias $i="rn $i"
done

for ((i = 0; i < 10000; i++)); do
  alias f$i="fn $i"
done

for ((i = 0; i < 10000; i++)); do
  alias a$i="an $i"
done

# }}}

#}}}

#### General Developement {{{

# General Navigation {{{

export DS=/KIWI/datasets
export CDS=/KIWI/datasets/Tailim/Paju
export MODE=VUE

# Selecting the development mode
mode() {
  menu "echo 'VUE\nMAP\nSCD'"
  crc MODE $menu
}

# Go to projects directory, depending on the development mode
pj() {
  cd $PJ_ROOT
  if [[ -d $PJ ]]; then
    o $PJ
  fi
}

pjr() {
  o $PJ_ROOT
}

# Go to the head project directory, depending on the development mode
hd() {
  cd $PJ_ROOT
  if [[ -d $HD ]]; then
    cd $HD
  fi
}

# }}}

# Data Set {{{

# Data Set: Change Data Set
# cds : Change data set
cds() {
  wtl "Changing dataset"
  crc CDS $(pwd)
  rm /kiwi/data
  sc $(pwd)/MAP /kiwi/data
  jdi $1
  wtu "Changing dataset [DONE]"

#  if [[ "$TARGET" = *.sql ]]; then
#    ln -s $DS/sql /kiwi/data
#    TARGET_DB=$(echo $TARGET | sed s/.sql//)
#    KWSQL_FILE=$DS/sql/kwsql
#    rm $KWSQL_FILE
#    echo "DATA=$TARGET" >> $KWSQL_FILE
#    echo "HOST=localhost" >> $KWSQL_FILE
#    echo "INTERFACE=sql" >> $KWSQL_FILE
#    echo "LOG=error" >> $KWSQL_FILE
#    echo "LOGPID=1" >> $KWSQL_FILE
}

# Data Set: Data Sets
# ds : Go to the data sets directory
ds() {
  o $DS
}

# Data Set: MAP Data Set
# mds : Go to the MAP data set
mds() {
  o $CDS/MAP
}

# Data Set: Java Data Set
# vds : Go to the Java data set
vds() {
  o $CDS/VUE
}



# }}}

# Workflow {{{

upall() {
  pj
  svn up
  jats
  upjava
  upmap
}

# }}}

# Servers {{{

# Server : Haoyangnz
# hynz : Connect to haoyangnz
hynz() {
	ssh haoyangnz.ddns.net
}

# Server : Calypso
# calypso : Go to the mounted server calypso
calypso() {
  dm calypso
}

angeline() {
  ssh angeline.loh@nzdevangelinel2
}

# Login to nzboom
# If $1 is defined then upload the file to nzboom shared folder
boom() {
  if [[ $1 = "" ]]; then
    ssh nzboom
  elif [[ -d $1 ]]; then
    if [[ $2 = "" ]]; then
      scp -r $1 nzboom:~/shared
    else
      scp -r $1 nzboom:~/shared/$2
    fi
  else
    if [[ $2 = "" ]]; then
      scp $1 nzboom:~/shared
    else
      scp $1 nzboom:~/shared/$2
    fi
  fi
}

support() {
# tucan4
  ssh remuser@kiwidev
}

# Download from boom
# If $1 is defined then download the file to nzboom shared folder
boomdl() {
  dc boom
	rmu 'nzboom:~/shared/*' .
}

#TODO Doesn't work
gromblecollect() {
  cdd gromble
  rm -rf *
  scp -r 'gromble:~/support/nzcollect/$1' .
}

gromble() {
  sshpass -p $PASS ssh gromble $@
}

release() {
  if [[ $1 = "" ]]; then
    gromble "find /support/support/revisions/TEST_ONLY/refresh/"
  else
    sshpass -p $PASS scp -r ${@:2} gromble:/support/support/revisions/TEST_ONLY/refresh/$1
  fi
}

# Release : All
# releaseall "ssd@nzodie:/src/kiwi_7.91_01mar2014/linux_2.12_kiwi_7.91_140301.140331.rev.tar.gz" 0228
releaseall() {
# MAP Revision
  gromble "rm -rf /support/support/revisions/TEST_ONLY/refresh/classic/*"
  work release
  r *
  scp $1 .
  release classic *

# VUE Data
  gromble "rm -rf /support/support/revisions/TEST_ONLY/refresh/vue/data/*"
  vds
  release vue/data *$2*

# VUE Revision
  gromble "rm -rf /support/support/revisions/TEST_ONLY/refresh/vue/*.sh"
  jid
  release vue *.sh
  gromble "rm -rf /support/support/revisions/TEST_ONLY/refresh/vue/windows/*"
  jwid
  release vue/windows windows/*

  release
}

# suboom : Login to nzboom as ssd
suboom() {
  ssh ssd@nzboom
}

# odie : Login to nzodie
odie() {
  ssh ssd@nzodie
}

# 
kpi() {
#  ssh kpi@maven "./haoyang_kpi_export.sh"
  r $COMMON/kpi/*
  scp "kpi@maven:/home/kpi/haoyang/*.csv" $COMMON/kpi
#  ooffice $COMMON/kpi/*.csv
}

# Login to aurora
aurora() {
  o $MNT/aurora
}

# Demo machine
demo() {
  ssh ssd@nzjavademomsg
}

sci() {
  ssh-copy-id $1
}

# }}}

# SQL {{{

# Run SQL with credentials and database
sql() {
  if [[ $1 = "" ]]; then
    DB=""
  else
    DB=${SQL_PREFIX}$1
  fi
  mysql -uroot -proot $DB "${@:2}"
}

# Find table in databases
sqlf() {
  for database in $(mysql -uroot -proot -e "show databases" | g $SQL_PREFIX); do
    if table=$(mysql -uroot -proot $database -e "show tables" | g $(pab $1)); then
      e $database
      e $table
      e
    fi
  done
}

# SQL : SQL List Database
# sqlld : List all databases under the SQL_PREFIX
sqlld() {
  mysql -uroot -proot -e "show databases" | g "^$SQL_PREFIX" | s "s/^$SQL_PREFIX//"
}

# Rename SQL database
sqlmv() {
  mysql -uroot -proot -e "create database ${SQL_PREFIX}$2"
  for table in `mysql -uroot -proot -B -N -e "show tables" ${SQL_PREFIX}$1`; do 
    mysql -uroot -proot -e "rename table ${SQL_PREFIX}$1.$table to ${SQL_PREFIX}$2.$table"
  done
  mysql -uroot -proot -e "drop database ${SQL_PREFIX}$1"
}

# Drop SQL database
sqlrm() {
  mysql -uroot -proot -e "drop database "${SQL_PREFIX}$1""
}

# Create SQL database
sqlmk() {
  mysql -uroot -proot -e "create database "${SQL_PREFIX}$1""
}

# Import SQL script to database
# $2 database dump suffix
# sqli tailim : Imports mes_8_csc/man_tailim.sql to mes_8_csc/man if java revision is mes-8
sqli() {
  if [[ $1 != "" ]]; then
    sql_suffix=_$1
  else
    unset sql_suffix
  fi
  for db in $(sqlld); do
    sqlfile=$SQL_PREFIX$db$sql_suffix.sql
    if [[ -f $sqlfile ]]; then
      sqlrm $db
      sqlmk $db
      sql $db < $sqlfile
    else
      e Cannot find $sqlfile
    fi
  done
  wtt "sqli [DONE]"
}

# Export SQL database to script
# $1 database dump suffix
# sqlo tailim : Exports mes_8_csc/man to mes_8_csc/man_tailim.sql if java revision is mes-8
sqlo() {
  if [[ $1 != "" ]]; then
    SQL_SUFFIX=_$1
  fi
  for db in $(sqlld); do
    sqlfile=$SQL_PREFIX$db$SQL_SUFFIX.sql
    mysqldump --skip-tz-utc -uroot -proot $SQL_PREFIX$db > $sqlfile
  done
  wtt "sqlo [DONE]"
}

# Show SQL process list
sqlp() {
  mysql -uroot -proot -e "show processlist"
}

# Execute SQL in database
sqle() {
  table=$(echo $@ | sed "s/^.*\(from\|desc\|update\) //" | cut -d " " -f1)
  database=$(sqlf $table | pl 1 1)
  eval "mysql -uroot -proot $database -e \"$@\""
}

# SQL Utility : SQL Query
# sqlq "Query" : query the database with Simplified-SQL
sqlq() {
  if [[ $1 == "" ]]; then
    while read table; do
      sqlqe $table
    done
  else
    sqlqe $table
  fi
}
sqlqe() {
  result=$(sqlf $1)
  database=$(e $result | pl 1 1)
  tables=$(e $result | pl 2)
  if ! table=$(e $tables | g ^$1$); then
    table=$(e $tables | pl 1 1) # Use the first match if no exact match
  fi
  count=$(sqle "select count(*) from $table" | head -3 | tail -1)
  (( column = $(sqle "desc $table" | wc -l) - 1 )) 
  (e $table@$database: $count rows and $column columns;
   sqle "select * from $table \G") | vi -
}

# SQL Utility: Explain a SQL table
# sqlt
sqlt() {
  if [[ $1 = "" ]]; then
    while read table; do
      sqlte $table
    done
  else
    sqlte $1
  fi
}
sqlte() {
  database=$(sqlf $1)
  mysql -uroot -proot $database -e "select * from $1 limit 1 \G;" | vi -
}

# }}}

# SVN {{{
unsvn () {
  find . -name ".svn" | xargs /bin/rm -rf
}

reapply() {
  dir=$(pwd)
  cd ..
  rm -rf $dir
  mv $dir.bak $dir
  cd $dir
  dp
}

revert() {
  dir=$(pwd)
  cd ..
  mv $dir $dir.bak
  svn up
  cd $dir
  dp
}

# SVN Revert Utility
# rv : Revert all changes and remove unversioned files
rv() {
  if [[ -d .git ]]; then
    git reset --hard
  fi
  if [[ -d .svn ]]; then
    svn -R revert .
    svn st
    svn st | g \? | awk '{print $2}' | xargs rm -rf
    svn st
  fi
}

# SVN Switch Utility
# sw 7.72 : Switch the current working copy to branch 7.72
sw() {
  if [[ -d .git ]]; then
		git checkout $1
		br
	fi
  if [[ -d .svn ]]; then
		svn sw $(svn info | g URL | cut -d " " -f 2 | sed -r 's/branches\/[^\/]*\//branches\/'$1'\//')
	fi
}

# SVN Rollback
# rb 1000 1004 : Roll back changes from revision 1000 to 1004
rb() {
  if [[ $2 == "" ]]; then
    svn merge -c -$1 .
  else
    svn merge -r $2:$1 .
  fi
}

# SVN Merge Utility
# mg 7.72 8399 : Merge r8399 from branch 7.72 to the working copy
mg() {
  if [[ -d .git ]]; then
		git merge $1
	fi
  if [[ -d .svn ]]; then
		if [[ $2 == "" ]]; then
			svn merge -c $1 .
		else
			svn merge -c $2 $(svn info | g URL | cut -d " " -f 2 | sed -r 's/branches\/[^\/]*/branches\/'$1'/')
		fi
	fi
}

# SVN Merge Out Utility
# mo : Merge out the changes from head branch to base
mo() {
  crc MMB $MHD
  crc JMB $JHD
  pj
  svn merge $PJ_ROOT/$MB
}

# SVN Merge In Utility
# mi : Merge in the changes from base to head branch
mi() {
  crc MMB $MPJ
  crc JMB $JPJ
  hd
  if [[ $1 = "" ]]; then
    svn merge --reintegrate $PJ_ROOT/$MB
  else
    svn merge -c r$1 $PJ_ROOT/$MB
  fi
}

# SVN Tree Conflict: List Add Conflicts
# tca : List tree conflicts for "local add, incoming add"
tca() {
  svn st | g ">   local add, incoming add upon merge" -B1 | gv ">" | gv "\-\-" | cut -c9- | nl
}

# SVN Tree Conflict: Resolve Identical Add Conflicts
# tcar : Resolve tree conflicts for "local add, incoming add" by accepting local, where the files are identical
tcar() {
  for tc in $(svn st | g ">   local add, incoming add upon merge" -B1 | gv ">" | gv "\-\-" | cut -c9-); do
    if [[ "$(diff $tc $PJ_ROOT/$MB/$tc)" = "" ]]; then
      svn resolve --accept working $tc
    fi
  done
}

# SVN Tree Conflict: Accept Incoming
# tcaa : Accept incoming files for tree conflicts for "local add, incoming add"
tcaa() {
  for tc in $(svn st | g ">   local add, incoming add upon merge" -B1 | gv ">" | gv "\-\-" | cut -c9-); do
    cp $PJ_ROOT/$MB/$tc $tc
  done
}

# SVN Tree Conflict: List Other Conflicts (Non-add)
# tco : List Tree conflicts other than "local add, incoming add"
tco() {
  svn st | gv ">   local add, incoming add upon merge" | g ">()" -B1 | gv ">" | gv "\-\-" | cut -c9-
}

svncomment() {
  svn propset svn:log --revprop -r $1 $2 $SVN
}

svnlog() {
  if [[ $1 = "" ]]; then
    svn log | head -5000 | head -30
  else
    svn log | head -5000 | g -C2 $1 | head -30
  fi
}

svndi() {
  svn di -c $1 *
}

br() {
	if [[ -d .git ]]; then
		pn br "git branch --color -a $@"
	fi
}

bru() {
	if [[ -d .git ]]; then
		git push -u origin $(brcur)
	fi
}

brt() {
	if [[ -d .git ]]; then
		git checkout -b $1 origin/$1
		br
	fi
}

brn() {
	if [[ -d .git ]]; then
		git checkout -b $@
	fi
	br
}

# Repository Branch : Delete
brd() {
	if [[ -d .git ]]; then
		if [[ $1 == $(brcur) ]]; then
			# Trying to delete current branch, switch to master first
			git checkout master
			git branch -D $@
		else
			git branch -D $@
		fi
	fi
	br
}

# Repository Branch : Current 
# Get current branch name
brcur() {
	if [[ -d .git ]]; then
		git branch | grep ^\* | cut -d" " -f2
	fi
}

# Repository History : Simple
hss() {
  if [[ -d .git ]]; then
    git log --graph --full-history --all --color --abbrev-commit --decorate --date=relative --format=format:'%C(blue)%h%C(reset) - %C(green)(%ar)%C(reset) %C(bold red)%s%C(reset) %C(cyan)- %an%C(reset)%C(yellow)%d%C(reset)' --simplify-by-decoration $@
  fi
}
	
hs() {
  if [[ -d .git ]]; then
    git log --graph --full-history --all --color --abbrev-commit --decorate --date=relative --format=format:'%C(blue)%h%C(reset) - %C(green)(%ar)%C(reset) %C(bold red)%s%C(reset) %C(cyan)- %an%C(reset)%C(yellow)%d%C(reset)' $@
  fi
}

st() {
  if [[ -d .git ]]; then
    git status
  fi
  if [[ -d .svn ]]; then
    pn st "svn status"
  fi
}

stm() {
  svn st | g ^M
}

# Repository : Diff Simple
# rds : Show simple repository diff
rds() {
	if [[ -d .git ]]; then
		git diff $@
	fi
}

dt() {
  if [[ -d .git ]]; then
    git difftool $@
  fi
  if [[ -d .svn ]]; then
    svn diff $@
  fi
}

cia() {
  if [[ -d .git ]]; then
    git commit -a --amend -m $1
  fi
}

todo() {
	gr "TODO "
}

ci() {
  if [[ -d .git ]]; then
    git commit -a -m $1
		git push
  fi
  if [[ -d .svn ]]; then
    if [[ $(svn st | g "^C") = "" ]]; then
      svn ci ${@:2} -m $1
    else
      echo ci: Couldn\'t commit due to following conflicts
      svn st | g "^C"
      return 1
    fi
  fi
}

cip() {
  ci $1
  if [[ -d .git ]]; then
    git push
  fi
}

co() {
  wtl "Check out"
  if [[ $1 == *git* ]]; then
    git clone $1
  fi
  if [[ $1 == *svn* ]]; then
    svn co $1
  fi
  wtu "Check out [DONE]"
}

up() {
  if [[ -d .git ]]; then
    git pull
  fi
  if [[ -d .svn ]]; then
    svn up
  fi
}

svnst() {
  svn di -r 31094:31095 $SVN/trunk/$1 | g Index | cut -d " " -f2
}

svndil() {
  for file in $(svnst $(rnode $(pwd) / 0)); do vimdiff $1/$file $file; done
}

# Blame
bl() {
  svn blame $(find . -name $1) | less
}

# }}}

# Misc Navigation {{{

# Directory : Common
# dc osm : Go to the osm common folder
dc() {
  d $COMMON/$1
}

# Directory : Mount
# dm calypso : Go to the mounted directory calypso
dm() {
  d $MNT/$1
}

# Go to the desktop work directory
work() {
  o $MOS_ROOT/work
  if [[ $1 != "" ]]; then
    d $1
  fi
}

# Sandbox: Sandbox
# sb : Go to the Sandbox directory
sb() {
  o $SANDBOX
}

# Edit note
note() {
  cd $NOTE
  if [[ $1 = "" ]]; then
    t -rt
  else
    vi $1
  fi
}

# }}}

#}}}

#### Java Development {{{

export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_45.jdk/Contents/Home

# Java Navigation {{{

export JPJ_ROOT=~/projects
#export JPJ=mes-excl-8.10
export JPJ=mes-excl-8.14
export JPD=$(lnode $JPJ - 1)
MERGEREV=("mes-8.10" "mes-8.10.1") export MERGEREV
export JREV=$(e $JPJ | cut -c5-)
export JHD=mes-8.0
export JMB=mes-8.0
export SITE_NAME=$(echo $JPJ | sed "s/[-,\.]/_/g")
export VDS=$CDS/VUE
export REBEL_HOME=~/.IdeaIC11/config/plugins/jr-ide-idea/lib/jrebel
export SV=/kiwi/java/sites
export CM=/kiwi/java/comms
export TC=/kiwi/java/tomcat
export WK=/kiwi/java/work
export IN=~/installers
export JSVN="svn+ssh://corona2/svn/mapjava"
if [[ $MODE == "VUE" ]]; then
  export SQL_PREFIX=${SITE_NAME}_
  export SVN=$JSVN
  export PJ_ROOT=$JPJ_ROOT
  export PJ=$JPJ
  export HD=$JHD
  export MB=$JMB
  export SVN=$JSVN
fi

alias vue="mode <<< 1"

# Go to java service directory
sv() {
  o $SV
  if [[ ! -d $SITE_NAME ]]; then
    wtt "sv not found"
    return 1
  fi
  o $SITE_NAME
  o current
  wtt sv
} 

# Go to java service logs directory
svl() {
  o $SV
  o $SITE_NAME
  o current
  o logs
  wtt svl
} 

# Go to java tomcat directory
tc() {
  o $TC/$SITE_NAME/current
}

# Go to java comms directory
cm() {
  cd $CM
  cd $SITE_NAME
}

# Go to java work directory
wk() {
  cd $WK
  cd $SITE_NAME
}

sss() {
  if [[ $(pf jvm java jini | wc -l) == 0 ]]; then
    e DOWN
  elif [[ $(pf jvm java tomcat jini | wc -l) == 1 ]]; then
    e UP
  else
    e STARTING
  fi
}

ssse() {
  scrond "sss > $MOS_TMP/sss"
}

# Start java services
sts() {
  pwd=$(pwd)
  mwk
  evn $SV/$SITE_NAME/current/bin/startservers.sh
  o $pwd
}

# Stop java services
sps() {
  pk jvm java $SITE_NAME
}

dps() {
  dp
  ss
}

# Start/Stop java services
ss() {
  if [[ $MODE == "SCD" ]]; then
    pj
    o src
		wt ss
		nodemon server.js
		wt
  else
    sss=$(decolor <<< $(sss))
    case $sss in
      DOWN) sts;;
      *) sps; sts;;
    esac
  fi
}

# Start Scheduler
# ssc : Start TSS scheduler
ssc() {
  sv
  wtt ssc
  bin/runpcstssscheduler.sh
  wtt "ssc[DONE]"
}


# Go to java installers directory
jind() {
  o $IN
  if [[ -d $SITE_NAME ]]; then
    o $SITE_NAME
  fi
}

# rmsite : Removes current java revision installation
rmsite() {
  rm -rf $SV/$SITE_NAME
  rm -rf $WK/$SITE_NAME
  rm -rf $TC/$SITE_NAME
	rmsitedb
}
rmsitedb() {
  for db in $(sqlld); do
    sqlrm $db
  done
}

# Java Development: Log
# lg : Tail logs
lg() {
  if [[ $1 = "" ]]; then
    tail -f logs/*
  else
    tail -f logs/$1*.log.txt
  fi
}

# Java Development: Log Error
# lge : Tail logs and monitor for errors
lge() {
  lg | g Exception
}

# Start tomcat debug
dbt() {
  pk tomcat
  export JPDA_ADDRESS=13066
  export JPDA_TRANSPORT=dt_socket
  tc
  bin/catalina.sh jpda run
}

# Edit java fix time
fixtime() {
  vi $SV/$SITE_NAME/current/conf/kiwiplan/time.properties;
  cp $SV/$SITE_NAME/current/conf/kiwiplan/time.properties $TC/$SITE_NAME/current/kiwiconf/kiwiplan/time.properties;
}

# View latest java comms response
cmr() {
  cat kiwi_to_host/$(ls kiwi_to_host -rt | tail -1)
}

# Clear java comms folders
cmc() {
  rm host_to_kiwi/* <<< "y"
  rm kiwi_to_host/* <<< "y"
  rm archive/* <<< "y"
}

# Place java comms request
cmp() {
  cp $1 host_to_kiwi
}

# }}}

# Java Setup {{{

# Java Debug: Debug Java service
# jdb : Pick Java Service to debug
jdb() {
  pn jdb "ls $SV/$SITE_NAME/current/conf/kiwiplan/jini/*.xml"
}

# Java Setup: Check out Java project
# jco mes-8.0 : Check out mes-8.0 projects
jco() {
  vue
  pjr
  mvn project:workspace << EOF
$SVN/projects/$1

Y
EOF
}

jpj() {
  vue
  menu 'svn ls $SVN/projects | gv master | g '$1' | sed "s/\///"'
  crc JPJ $menu
  if [[ -d $PJ_ROOT/$PJ ]]; then
    pj
  else
    jco $menu
  fi
}

# Copy universal java licence file
# cplic csc : Copy CSC licence to the current directory
cplic() {
  cp $COMMON/licence/$1.licence .
}

jsv() {
  sps
  vue
  menu 'svn ls $SVN/projects | gv master | g '$1' | sed "s/\///"'
  crc JPJ $menu
  if ! sv; then
    jin
  fi
  #jdi
  ss
}

jstss() {
	cp $COMMON/roadgrids/* $SV/$SITE_NAME/current/conf/kiwiplan/roadgrids
  tc
  d webapps/kp-tss-map-tiles/maptiles
  sudo mount nzmaptiles.kiwiplan.co.nz:/data/default  .
}

jtst() {
	if [[ $1 == "" ]]; then
		maven build-all || cat target/test-reports/*.txt
	else
		testclass=$(find testsrc -name "$1.java" | cut -d "." -f1 | s 's/^testsrc\///g')
		mvn test -Dtest=$testclass
	fi
}


jup() {
  sps
  jid

# Run installation
  ./$JPJ-*.sh << EOF

$SITE_NAME

 
EOF
  jstss
  sv
  sts
}

jid() {
  d $IN/$JPJ
  rmu 'installers@nzjenkins:/data/installers/latestsingleinstaller/'$(echo $JPJ | sed "s/-[^-]*$//")/ . $JPJ'-*'
  cplic $JPD
}

jwid() {
  d $IN/$JPJ
  if [[ -d windows ]]; then
    rm -rf windows
  fi
  scp -r 'installers@nzjenkins:/home/installers/latest/'$JPJ windows
}

# Import SQL script to database
# $2 database dump suffix
# sqli tailim : Imports mes_8_csc/man_tailim.sql to mes_8_csc/man if java revision is mes-8
jdi() {
	jdr $1
	sv
	bin/upgrade.sh
	vds
	sqlo $1
}

jdr() {
	vds
  sqli $1
  if [[ -f time.properties ]]; then
    cp time.properties $SV/$SITE_NAME/current/conf/kiwiplan/time.properties
    cp $SV/$SITE_NAME/current/conf/kiwiplan/time.properties $TC/$SITE_NAME/current/kiwiconf/kiwiplan/time.properties
	fi
}

jinmes() {
  echo "### Basic
# Change base?
n
# Site name
$SITE_NAME
# Offset
$OFFSET
# Install
n
# Admin password
admin1
n
### Database
# Hostname
localhost
# Super user name
root
# Super user password
root
# Normal user name
root
# Super user password
root
# Metric
y
$METRIC
n
### Comms
n
# CSC
# Change database name?
n
# Database failed
#
### Manufacturing
# Change database name?
n
# Database failed
#
### Completed
# Configure
e
" | gv "#" | ./$JPJ-*.sh
}

jintss() {
  mv tss.licence kansas.licence #TODO generate a TSS licence
  echo "### Basic
# Change base?
n
# Site name
$SITE_NAME
# Offset
$OFFSET
# Install
n
# Admin password
admin1
n
### Database
# Hostname
localhost
# Super user name
root # Super user password
root
# Normal user name
root
# Super user password
root
# Metric
y
$METRIC
n
### Manufacturing Classic DB
#Change database name?
y
${SQL_PREFIX}map
n
### TSS integration
# Unit size calculator
n
# PCS scheduler configuration
y
/kiwi

n
# TSS DB
n
### Completed
# Configure
e
" | gv "#" | ./$JPJ-*.sh



#  TODO PORT PROBLEM?

  jstss

#  TODO Change pcs properties
}

# TODO Metric depends on current dataset, cds should configure service
# Java Setup: Fresh Service Installation
# jin : Do a fresh installation of the current java revision
jin() {
  rmsite
  jid
  METRIC=n
  OFFSET=0
  case $JPD in
    mes) jinmes;;
    tss) jintss;;
  esac
}

# Java Setup: Debug Trim
# jdt : Turn on debug for trim
jdt() {
  sv
  cat $COMMON/haoyang/trim.log | s "s/SITE_NAME/$SITE_NAME/" >> conf/log4j.properties
}

jdp() {
	pj
  modified_projects=("${(@f)$(svn st | grep "^[A-Z] " | grep -v ^X | cut -d " " -f8 | sed "s/src.*//g" | sed "s/test$//g" | sed "s/conf.*//g" | sed "s/pom.xml//g" | sed "s/file.*//g" | sort | uniq)}")
	e
	e Projects modified:
	e
	for project in $modified_projects; do
		echo "* $project"
	done
	e
	e ENTER to proceed, C-c to cancel
	e
	read
	for project in $modified_projects; do
		pj
		cd $project
		dp
	done
}

jja() {
	pj
  modified_projects=("${(@f)$(svn st | grep "^[A-Z] " | grep -v ^X | cut -d " " -f8 | sed "s/src.*//g" | sed "s/test$//g" | sed "s/conf.*//g" | sed "s/pom.xml//g" | sed "s/file.*//g" | sort | uniq)}")
	e
	e Projects modified:
	e
	for project in $modified_projects; do
		echo "* $project"
	done
	e
	e ENTER to proceed, C-c to cancel
	e
	read
	for project in $modified_projects; do
		pj
		cd $project
		e;e;e;e;e;
		e --------------------- BUILDING $project ------------------------
		e;e;e;e;e;
		ja || break
		e;e;e;e;e;
		e --------------------- FINISHED BUILDING $project ------------------------
		e;e;e;e;e;
	done
}

# Java Commit
# jci "Comment" : Commit with "Comment" for the current rev, merging to revisions in $MERGEREV
jci() {
  wtl "Committing"
  pj
  up
  externals=("${(@f)$(stm | cut -c9- | cut -c-3)}")
  ci $1 $externals
  up
  o ${externals[1]}
  merge_to_rev=$(svnlog haoyang | head -2 | tail -1 | cut -d " " -f 1 | cut -c2-)
  for rev in $MERGEREV; do
    e --- Merging to $rev..
    pjr
    o $rev
    up
    for external in $externals; do
      o $external
      mg $JREV $merge_to_rev
      o ..
    done
    ci $1 $externals
    up
  done
  wtu "Committing [DONE]"
}


# }}}

#}}}

#### MAP Development {{{

# MAP Navigation {{{

export MAP_REV_SRC=boom_base
export MAP_REV=8.00_01oct2014
export MAP_DATA_REV=01oct2014
export MAP_TRUNK=trunk
export MAP_BRANCH=dev_branches/messaging
export MPJ_ROOT=~/projects/map
export MPJ=kiwi_riegelsville
export MHD=kiwi_head
export MMB=kiwi_riegelsville
export MSVN="svn+ssh://corona2/svn/map"
if [[ $MODE == "MAP" ]]; then
  export SQL_PREFIX=""
  export SVN=$MSVN
  export PJ_ROOT=$MPJ_ROOT
  export PJ=$MPJ
  export HD=$MHD
  export MB=$MMB
  export SVN=$MSVN
fi

case $MAP_REV_SRC in
boom_base)
  export MAP_REV_SRC_SERVER=nzboom
  export MAP_REV_SRC_DIR=/src
  ;;
boom_haoyang)
  export MAP_REV_SRC_SERVER=nzboom
  export MAP_REV_SRC_DIR=~/projects
  ;;
aurora)
  export MAP_REV_SRC_SERVER=aurora
  export MAP_REV_SRC_DIR=/src
  ;;
esac

export MDS=$CDS/MAP
export KIWIPROGS=/kiwi/progs
export KIWISQL=/kiwi/sql
export KIWISCP=/kiwi/scp
export KIWIBIN=/kiwi/bin
export KIWIETC=/kiwi/etc
export KIWIWORK=/kiwi/work
export KIWISEA=$MDS:$KIWIPROGS:$KIWISQL:$KIWISCP:$KIWIBIN:$KIWIETC:$KIWIWORK
export LD_LIBRARY_PATH=$KIWIPROGS
export KWSQL_USER=test
export KWSQL_PASS=test


alias map="mode <<< 2"

# Go to MAP work directory
mwk() {
  o $KIWIWORK
}

# Restore MAP QA ISAM dataset
rest() {
  mds
  r *
  scp -r "ssd@aurora:/mapqa/master_$MAP_DATA_REV/test$1/*" .
}

# Restore MAP QA SQL dataset
restsql() {
  DIR=$DS/rest/$MAP_DATA_REV
  if [[ ! -f $DIR/test$1.sql ]]; then
    scp ssd@aurora:/mapqa/master_$MAP_DATA_REV/test$1.sql $DIR
  fi
  lndata $DIR/test$1.sql
}


jip() {
  mwk
  jimportpaper
}
jib() {
  mwk
  jimportboard
}
jih() {
  mwk
  jimporthistory
}
jio() {
  mwk
  jimportorder
}
jiq() {
  mwk
  jimportprogset
}
pcs() {
  mwk
  wtt pcs
  pcsmenu
}
csc() {
  mwk
  cscmenu
}
xm() {
  mwk
  xmgen grp=$1
}
ult() {
  mwk
  ult00
}
rss() {
  mwk
  rss01
}
xl() {
  mwk
  wtt xl
  echo -e "secr8\n" > .secr8
  xlmain -i .secr8
  rm .secr8
}

# }}}

# MAP Setup {{{

# Update MAP rev
upmap() {
  cd /KIWI/revisions
  if [[ -d kiwi_$MAP_REV ]]; then
    rm -rf kiwi_$MAP_REV.bak
    mv kiwi_$MAP_REV kiwi_$MAP_REV.bak
  fi
  mkdir kiwi_$MAP_REV
  cd kiwi_$MAP_REV
  scp -r ssd@$MAP_REV_SRC_SERVER:$MAP_REV_SRC_DIR/kiwi_$MAP_REV/progs progs
  scp -r ssd@$MAP_REV_SRC_SERVER:$MAP_REV_SRC_DIR/kiwi_$MAP_REV/scp scp
  scp -r ssd@$MAP_REV_SRC_SERVER:$MAP_REV_SRC_DIR/kiwi_$MAP_REV/sql sql
  scp -r ssd@$MAP_REV_SRC_SERVER:$MAP_REV_SRC_DIR/kiwi_$MAP_REV/etc etc
  scp -r ssd@$MAP_REV_SRC_SERVER:$MAP_REV_SRC_DIR/kiwi_$MAP_REV/bin bin
}

# Change MAP rev
maprev() {
  menu "echo 'boom_base\nboom_haoyang'"
  crc MAP_REV_SRC $menu
  menu "ssh $MAP_REV_SRC_SERVER ls -1d '$MAP_REV_SRC_DIR/kiwi_*' | sed 's/^.*kiwi_//g'"
  sed -i -r 's/^export MAP_REV.*$/export MAP_REV='$menu'/gi' $ZSHRC
  source $ZSHRC
  rm /kiwi/kiwilink
  if [[ ! -d /kiwi/revisions/kiwi_$menu ]]; then
    upmap
  fi
  ln -s /kiwi/revisions/kiwi_$menu /kiwi/kiwilink
}

# Change MAP rev for dataset restores
mapdatarev() {
  menu "ssh ssd@felix ls -1d '/mapqa/master_*' | cut -d _ -f 2"
  sed -i -r 's/^export MAP_DATA_REV.*$/export MAP_DATA_REV='$menu'/gi' $ZSHRC
  source $ZSHRC
}

# MAP Setup: MAP Work Cleanup
# mwc : Clean up MAP work directory by removing temp files
mwc() {
  rm *.LS
  rm *.TM
  rm *.XXX
  rm *.LOG
  rm *.log
  rm *.LG
  rm *.LG1
  rm *.OK
  rm *.LK
  rm *.lk
  rm *.TXT
  rm *.txt
  rm trim.log.*
  rm *.out
  rm core
  rm SQLERROR.*
}

# }}}

# MAP Workflow {{{

mgmess() {
  m
  pj && rv && up
  svn merge -c 11111 $SVN/trunk
  echo pj
  hd && rv && up
  echo hd
}

# }}}

# MAP Build {{{

# MAP Build: MAP Continuous Integration
# mci : Show the MAP continuous integration status
mci() {
  ssh nzboom ls -lrt /src/CI/logs
}

#}}}
 
#}}}

#### SCD Development {{{

# SCD Navigation {{{

export SPJ_ROOT="$WORK/sycoden"
export SPJ=ListSum
if [[ $MODE == "SCD" ]]; then
  export PJ_ROOT=$SPJ_ROOT
  export PJ=$SPJ
fi

alias scd="mode <<< 3"

# }}}

#}}}

#### General Application {{{

# Google Drive {{{

gd() {
  google docs edit --title $1 --folder Grive --format html --editor google_vim
}

# }}}

# Todo {{{

export TD_TODO=$NOTE/td/todo
export TD_DONE=$NOTE/td/done

# Todo Utility: Edit
# tde : Edit the TODO list
tde() {
  vi $TD_TODO
}

# Todo Utility: Add to the top of the TODO list
tdn() {
  task="$@"
  sed -i "1i$task" $TD_TODO
}

# Todo Utility: Add to the bottom of the TODO list
tda() {
  task="$@"
  echo $task >> $TD_TODO
}

# Todo Utility: Add to the middle of the TODO list
tdi() {
  task="${@:2}"
  sed -i $1i"$task" $TD_TODO
}

# Todo Utility: List tasks on the TODO list
tdl() {
  cat $TD_TODO | nl
}

# Todo Utility: Delete current task
tdd() {
  if [[ $1 = "" ]]; then
    line="1"
  else
    line=$1
  fi
  sed -i "$line"d $TD_TODO
}

# Todo Utility: Finish
# tdf : Finish current task
tdf() {
  ins $TD_DONE "$(catl $TD_TODO 1)"
  tdd
}

# Todo Utility: Undo finish
# tduf : Undo finish of previous task
tduf() {
  ins $TD_TODO "$(catl $TD_DONE 1)"
  si 1d $TD_DONE
}

# Todo Utility: List tasks on the DONE list
tdlf() {
  cat $TD_DONE | tail -30 | nl
}


# }}}

#}}}

#### Other {{{

# Experimental {{{

dj() {
  jarname=`ab $1`".jar"
  find ~/.maven/repository -name $jarname | xargs cp --target-directory .
}

# }}}

# Misc {{{
#export _JAVA_OPTIONS='-Dswing.defaultlaf=com.sun.java.swing.plaf.gtk.GTKLookAndFeel'
#export HISTCONTROL=ignoredups
#export _JAVA_OPTIONS="-Dawt.useSystemAAFontSettings=lcd"

PATH="$PATH:$MOS_BIN:$RC/bin"
if [[ -d "~/projects/maven-misc/bin" ]] ; then
    PATH="~/projects/maven-misc/bin:$PATH"
fi
if [[ -d "/usr/local/java/maven3/bin" ]] ; then
    PATH="/usr/local/java/maven3/bin:$PATH"
fi
if [[ -d "~/.local/bin" ]] ; then
    PATH="~/.local/bin:$PATH"
fi
#PATH="$MOS_BIN/Sencha/Cmd/3.0.0.250:$PATH"
PATH=$PATH:~/projects/maven-misc/bin
PATH=$PATH:/usr/sbin

PERL_MB_OPT="--install_base \"/home/haoyang.feng/perl5\""; export PERL_MB_OPT;
PERL_MM_OPT="INSTALL_BASE=/home/haoyang.feng/perl5"; export PERL_MM_OPT;

# }}}

#}}}

#### Dependencies {{{ 

# Install Software {{{

# TODO coreutils for BSD
case $OS in
	GNU) deps=(zsh urxvt tmux vim irssi elinks mutt-patched emacs git tree grc sshfs xclip ssh-copy-id);;
	BSD) deps=(zsh tmux vim irssi emacs git tree grc node neo4j ssh-copy-id);;
esac
for dep in $deps; do
	wn $dep || pi $dep
done

# }}}

# Directories {{{

md $MOS_TMP $MOS_TMP/stdout $MOS_TMP/stdbuf $MOS_TMP/stderr
md $MNT
md $TRASH $ARCHIVE

# }}}

# Mount {{{

if [[ ! -d $MNT/calypso ]]; then
  md $MNT/calypso
  rmm calypso:/data/kiwiplan/docs/TSS $MNT/calypso
fi

if [[ ! -d $MNT/aurora ]]; then
  md $MNT/aurora
  rmm ssd@aurora:/ $MNT/aurora
fi


# }}}

# }}}

#### Startup {{{ 

# Setup Environment {{{

case $OS in
	GNU) eval $(dircolors ~/.dir_colors);;
	BSD) eval $(gdircolors ~/.dir_colors) && eval $(ls-colors-linux-to-bsd);;
esac

# }}}

# Goto home {{{

if $NEW; then
  cs
  echo
  d
fi
NEW=false

# }}}

#}}}
