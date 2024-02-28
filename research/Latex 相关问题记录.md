# Latex 相关问题记录

[annotation]: [id] (13b0bcfe-68ae-410b-aa96-7e279c25759e)
[annotation]: [status] (public)
[annotation]: [create_time] (2024-02-28 16:21:38)
[annotation]: [category] (我的日记)
[annotation]: [tags] (Latex)
[annotation]: [comments] (false)
[annotation]: [url] (http://blog.ccyg.studio/article/13b0bcfe-68ae-410b-aa96-7e279c25759e)

## 插入 svg

插入 svg 的过程略显复杂，首先需要包 svg [^svg]，其次需要 svg -> pdf 转换工具 inkscape [^inkscape]，inkscape 可以使用 pacman 直接安装。

然后输入 Latex 的图像类似如下：

```latex
\usepackage{svg}

...

\begin{figure}[h]
    \centering
    \includesvg{image.svg}
    \caption{Caption}
    \label{fig:label}
\end{figure}
```

另外如果在 vscode latexworkshop 中，则需要修改 `latexmk` 或 `xelatex` 添加命令行选项 `-shell-escape`，在 vscode 中如下所示，来自对默认值的修改，默认值在 [^default-value]：

```json
"latex-workshop.latex.tools": [
        {
            "name": "latexmk",
            "command": "latexmk",
            "args": [
                "-shell-escape",
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "-outdir=%OUTDIR%",
                "%DOC%"
            ],
            "env": {}
        },
        {
            "name": "lualatexmk",
            "command": "latexmk",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-lualatex",
                "-outdir=%OUTDIR%",
                "%DOC%"
            ],
            "env": {}
        },
        {
            "name": "xelatexmk",
            "command": "latexmk",
            "args": [
                "-shell-escape",
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-xelatex",
                "-outdir=%OUTDIR%",
                "%DOC%"
            ],
            "env": {}
        },
        {
            "name": "latexmk_rconly",
            "command": "latexmk",
            "args": [
                "%DOC%"
            ],
            "env": {}
        },
        {
            "name": "pdflatex",
            "command": "pdflatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOC%"
            ],
            "env": {}
        },
        {
            "name": "bibtex",
            "command": "bibtex",
            "args": [
                "%DOCFILE%"
            ],
            "env": {}
        },
        {
            "name": "rnw2tex",
            "command": "Rscript",
            "args": [
                "-e",
                "knitr::opts_knit$set(concordance = TRUE); knitr::knit('%DOCFILE_EXT%')"
            ],
            "env": {}
        },
        {
            "name": "jnw2tex",
            "command": "julia",
            "args": [
                "-e",
                "using Weave; weave(\"%DOC_EXT%\", doctype=\"tex\")"
            ],
            "env": {}
        },
        {
            "name": "jnw2texminted",
            "command": "julia",
            "args": [
                "-e",
                "using Weave; weave(\"%DOC_EXT%\", doctype=\"texminted\")"
            ],
            "env": {}
        },
        {
            "name": "pnw2tex",
            "command": "pweave",
            "args": [
                "-f",
                "tex",
                "%DOC_EXT%"
            ],
            "env": {}
        },
        {
            "name": "pnw2texminted",
            "command": "pweave",
            "args": [
                "-f",
                "texminted",
                "%DOC_EXT%"
            ],
            "env": {}
        },
        {
            "name": "tectonic",
            "command": "tectonic",
            "args": [
                "--synctex",
                "--keep-logs",
                "%DOC%.tex"
            ],
            "env": {}
        }
    ],
```

另外默认情况下 svg 会在命令当前目录生成 `svg-inkscape` 目录存储生成后的文件，如果想要修改此目录则可以在 `\includesvg` 中添加参数 `inkscapepath` [^inkscapepath]来修改，具体结果如下所示：

```latex
\begin{figure}[h]
    \centering
    \includesvg[inkscapepath=.latexout/svg-inkscape]{image.svg}
    \caption{Caption}
    \label{fig:label}
\end{figure}
```

## References

[^svg]: [svg](https://ctan.org/pkg/svg?lang=en)

[^inkscape]:[inkscape](https://inkscape.org/)

[^inkscapepath]:[修改 inkscape 目录](https://github.com/mrpiggi/svg/issues/11)


[^default-value]: <https://github.com/James-Yu/LaTeX-Workshop/blob/master/package.json>
