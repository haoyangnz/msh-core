# What is Msh?
Msh (My Shell) is a heavily customised shell built on top of zsh.

# Installing Msh
1. Make sure zsh is installed. (If you want msh to work on top of other shells, feel free to port it yourself. If you want msh to work as a standalone shell, let me know because I've been thinking about that too.)
2. Clone the repository to ~/msh. We strongly recommend you to fork this repository and clone your own fork, instead of cloning my repository, as the core value of Msh is to encourage community contribution. `mkdir ~/msh && cd ~/msh && git clone https://github.com/your-username/msh-core.git`
3. Install any Msh plugins that you need, similarly to the step 2 above. Again, we strongly recommend you to clone your own fork so that you can contribute easily. For example, `cd ~/msh && git clone https://github.com/your-username/msh-kiwiplan.git`, if you want the Msh Kiwiplan plugin. 
4. Source msh-core in your .zshrc. `echo ". ~/msh/msh-core/core.msh" > ~/.zshrc`
5. Msh works best within tmux. So install tmux.
6. We also recommend you to get the recommended dot files for a better experience. Clone https://github.com/haoyangnz/msh-dotfiles.git and link the dot files to your home directory. Please do not add this in the msh folder. msh directory should only contain msh modules.
7. We recommend running Msh with the "Anonymous Pro" font. Download the font from http://www.marksimonson.com/fonts/view/anonymous-pro put it into your font library. In many linux distros, you can put it in `~/.fonts`. Step 8 talks about using the font in your terminal emulator.
8. Tweak your terminal emulator: If you are using linux, it's best to run Msh with urxvt (rxvt-unicode), so install that. The msh dotfiles in step 6 would have included .Xresources which will configure the look and feel of urxvt. Please run `xrdb -merge ~/.Xresources` to load the Xresources file. If you are using Mac OS X, Terminal.app is pretty good, we recommend you configure the colour scheme to Solarized Light (see http://ethanschoonover.com/solarized), and use the Anonymous Pro font from step 7.

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
