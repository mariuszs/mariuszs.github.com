mariuszs.github.com
===================

Shell
----

I'm using [Informative git prompt](https://github.com/fish-shell/fish-shell/blob/master/share/functions/__fish_git_prompt.fish) in [Fish shell](http://fishshell.com/).

My prompt configuration (select *informative git prompt* from [fish_config](http://fishshell.com/docs/current/commands.html#fish_config)!):

    set -g __fish_git_prompt_show_informative_status 1

    set -g __fish_git_prompt_showcolorhints 1

    set -g __fish_git_prompt_color_branch magenta bold
    set -g __fish_git_prompt_showupstream "informative"
    set -g __fish_git_prompt_char_upstream_ahead "↑"
    set -g __fish_git_prompt_char_upstream_behind "↓"
    set -g __fish_git_prompt_char_upstream_prefix ""

    set -g __fish_git_prompt_char_stagedstate "●"
    set -g __fish_git_prompt_char_dirtystate "✚"
    set -g __fish_git_prompt_char_untrackedfiles "…"
    set -g __fish_git_prompt_char_conflictedstate "✖"
    set -g __fish_git_prompt_char_cleanstate "✔"

    set -g __fish_git_prompt_color_dirtystate blue
    set -g __fish_git_prompt_color_stagedstate yellow
    set -g __fish_git_prompt_color_invalidstate red
    set -g __fish_git_prompt_color_untrackedfiles $fish_color_normal
    set -g __fish_git_prompt_color_cleanstate green bold

My fish cow

    if status --is-interactive
        command fortune -s | cowsay
    end

looks like this

     ________________________________________
    < Beat me, whip me, make me use Windows! >
     ----------------------------------------
            \   ^__^
             \  (oo)\_______
                (__)\       )\/\
                    ||----w |
                    ||     ||
    ~/git $



Editor
----

I'm big fan of [nano editor](http://www.nano-editor.org/) 2.3.2 with [syntax highlighting for Git](https://raw.github.com/scopatz/nanorc/master/git.nanorc).

GIT
-----

I'm using almost latest GIT (`git version 1.8.4.2`) supplied by [Fedora](http://fedoraproject.org/).


Configuration (short version)

    [user]
        name = Mariusz Smykula
        email = mariuszs@gmail.com
    [core]
        editor = nano
    [color]
        ui = auto
    [branch]
        autosetuprebase = always
    [alias]
        undo = reset HEAD@{1}
    [apply]
        whitespace = nowarn

Other tools
----

 * [hub](http://hub.github.com/) - command-line wrapper for github.
 * [git kurwa](https://github.com/jakubnabrdalik/gitkurwa)
 * [Terminator terminal](http://gnometerminator.blogspot.com/p/introduction.html)
 * [tig](http://jonas.nitro.dk/tig/) - text-mode interface for Git