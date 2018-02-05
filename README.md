# What is Msh?
Msh (My Shell) is a heavily customised shell built on top of zsh.

# Installing Msh
1. Make sure zsh is installed. (If you want msh to work on top of other shells, you can give it a try - msh is mostly POSIX compliant - if anything doesn't work feel free to submit a pull request.)
2. Clone the repository to ~/msh. We strongly recommend you to fork this repository and clone your own fork, instead of cloning my repository, as the core value of Msh is to encourage community contribution. `mkdir ~/msh && cd ~/msh && git clone git@github.com:your-username/msh-core.git`
3. Source msh-core in your .shrc. `echo ". ~/msh/msh-core/core.msh" > ~/.zshrc`
4. Msh works best within tmux. So install tmux.
5. We also recommend you to get the recommended dot files for a better experience. Clone https://github.com/haoyangnz/msh-dotfiles.git and link the dot files to your home directory. Please do not add this in the msh folder. msh directory should only contain msh modules.
6. We recommend running Msh with the "Anonymous Pro" font. Download the font from http://www.marksimonson.com/fonts/view/anonymous-pro put it into your font library. In many linux distros, you can put it in `~/.fonts`. Step 7 talks about using the font in your terminal emulator.
7. Tweak your terminal emulator: If you are using linux, it's best to run Msh with urxvt (rxvt-unicode), so install that. The msh dotfiles in step 5 would have included .Xresources which will configure the look and feel of urxvt. Please run `xrdb -merge ~/.Xresources` to load the Xresources file. If you are using Mac OS X, Terminal.app is pretty good, we recommend you configure the colour scheme to Solarized Light (see http://ethanschoonover.com/solarized), and use the Anonymous Pro font from step 6.

# Launching Msh
Assuming you've done all the steps above:

1. Open your tweaked terminal emulator.
2. Run `zsh` to launch zsh, with msh configured on top.
3. Run `std` to launch tmux with the "standard" session configurations.
4. Start using Msh!

# Using Msh
Assuming you've done all the steps above:
- Ctrl-Alt-H, Ctrl-Alt-J, Ctrl-Alt-K, Ctrl-Alt-L: Toggles between four standard tmux sessions.
- Ctrl-Alt-Q, Ctrl-Alt-W, Ctrl-Alt-E, Ctrl-Alt-R, Ctrl-Alt-T, Ctrl-Alt-Y, Ctrl-Alt-U, Ctrl-Alt-I, Ctrl-Alt-O, Ctrl-Alt-P: Toggles between tmux tabs.

## Installing Msh Plugins
Install any Msh plugins that you need, by first forking the repository such as https://github.com/haoyangnz/msh-kiwiplan, if you want the Msh Kiwiplan plugin, then use the `mi` command, such as `mi your-username kiwiplan`.
