
# ChatGPT-Arcana

## About

🔮 ChatGPT-Arcana is an Emacs Lisp package that gives you arcane powers. Yer a space wizard now, Harry.

## Examples

#### Create and edit code
https://user-images.githubusercontent.com/859820/222489571-27901725-158e-4a2a-899a-36a3f4eea2c1.mp4

#### Write a doc about ffmpeg in org-mode
https://user-images.githubusercontent.com/859820/222561453-031d271f-4fee-46f4-b63e-734729e01745.mp4

#### Explain code in the selected region
https://user-images.githubusercontent.com/859820/222563046-5928a98d-7498-4bce-9916-f5a7d24acc81.mp4

#### Have a lovely chat
https://user-images.githubusercontent.com/859820/222718608-b767a663-86f9-4c56-acbe-192e1e91fe26.mp4

## Usage

There are various interactive functions available. Some of them even work.

This package, for now, provides the following functionality:

- Chat with GPT in Emacs with `chatgpt-arcana-chat-start-chat` (this is the best part of this package).
- Generate text content based on prompt and optionally selected region using `chatgpt-arcana-query`
- Replace selected text region with generated text content using `chatgpt-arcana-replace-region`
- Insert generated text at the current cursor position with informative context lines using `chatgpt-arcana-insert-at-point-with-context`
- Insert generated text at, after or before selected text region using `chatgpt-arcana-insert-after-region` and `chatgpt-arcana-insert-before-region` and `chatgpt-arcana-insert-at-point`
- Chat buffer auto-naming (modify prompt with custom var `chatgpt-arcana-generated-buffer-name-prompt`)
- Chat session autosave and automatic file naming (disable with custom var`chatgpt-arcana-chat-autosave-enabled`, modify save directory with `chatgpt-arcana-chat-autosave-directory`)

## Installation

ChatGPT-Arcana isn’t on melpa or elpa. You can use use-package to install from github:

```elisp
(use-package chatgpt-arcana
  :straight (:host github :repo "CarlQLange/ChatGPT-Arcana.el" :files ("*.el"))
  :init (setq chatgpt-arcana-api-key "your-api-key-here"))
```

You may wish to use `.authinfo` or similar to hold your API key. You can do this by creating or adding to your `~/.authinfo` file like the following:

```
machine chat.openai.com login YOUR_CHATGPT_EMAIL_HERE password YOUR_API_KEY_HERE
```

and using the line

```elisp
(setq chatgpt-arcana-api-key (auth-source-pick-first-password
                                              :host "chat.openai.com"))
```

in your config.

My own config adds a few extra parts that don't need to be part of the package.

```elisp
(use-package chatgpt-arcana
  :straight (:host github :repo "CarlQLange/ChatGPT-Arcana.el" :files ("*.el"))
  :init (setq chatgpt-arcana-api-key "your-api-key-here")
  :config 
  (use-package all-the-icons
    :config
    (add-to-list 'all-the-icons-mode-icon-alist
                 '(chatgpt-arcana-chat-mode all-the-icons-octicon "comment-discussion" :height 1.0 :v-adjust -0.1 :face all-the-icons-purple)))

  (use-package pretty-hydra
    :config
    (eval `(pretty-hydra-define chatgpt-arcana-hydra (:color blue :quit-key "q" :title "ChatGPT Arcana")
             ("Query"
              (("a" chatgpt-arcana-query "Query")
               ("r" chatgpt-arcana-replace-region "Replace region"))
              "Insert"
              (("i" chatgpt-arcana-insert-at-point-with-context "At point with context")
               ("I" chatgpt-arcana-insert-at-point "At point")
               ("j" chatgpt-arcana-insert-after-region "Before region")
               ("J" chatgpt-arcana-insert-before-region "After region"))
              "Chat"
              (("c" chatgpt-arcana-start-chat "Start chat"))
              "Shortcuts"
              (,@(chatgpt-arcana-generate-prompt-shortcuts))))))

  (map! :leader
        :prefix ("[" . "ChatGPT")
        :desc "Start chat" :g "c" #'chatgpt-arcana-start-chat
        :desc "Start chat" :g "[" #'chatgpt-arcana-start-chat
        :desc "Open Hydra" :g "h" #'chatgpt-arcana-hydra/body))
```

You are going to want to add a keymap of your own, that's for sure :)

I have to stress at this point that this package is very new, and I only wrote it to scratch an itch. Sorry if it turns you into a chicken or something.

## Requirements

You will need to have an API key from OpenAI’s GPT-3 language model to use this package, and set it as `chatgpt-arcana-api-key`. You can sign up for an API key on the OpenAI website. Depending on how much you use the package, the API cost may vary.

I strongly recommend setting a low usage limit on your account to stop runaway spending. The default model is quite cheap but it costs nothing to be prudent.

## Limitations

I have done exactly zero work in terms of compatibility. It works on my machine, sometimes. Maybe it'll even work on yours. Also, even though ChatGPT is pretty much the largest computing leap since the iPhone, it has its own limitations. Particularly, I've noticed it's pretty terrible at writing org-mode - I guess due to a lack of source data. Hence, the `chatgpt-arcana-chat-mode` is based on Markdown.

## Credits

This package was developed by Carl Lange with judicious help from ChatGPT.

Other contributors and contributions:
- GitHub user [macownersclub](https://github.com/macownersclub) for [PR #6](https://github.com/CarlQLange/chatgpt-arcana.el/pull/6)
- GitHub user [vxe](https://github.com/vxe) for authinfo tip in [issue #8](https://github.com/CarlQLange/chatgpt-arcana.el/issues/8)

## License

This package is licensed under the [GNU General Public License v3.0](https://www.gnu.org/licenses/gpl-3.0.en.html).

*Important note*: code generated by ChatGPT probably has a **massive license headache** attached to it and may result in RMS eating your dog.
