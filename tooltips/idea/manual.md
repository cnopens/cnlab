# Declare: this article 

#Idea use tips

## Most common use keyboard shortcuts...


## Focus plugin and develope it

1、emBrowser4Intellj


## Most popular Plugins
<ul style="">
	<li>Rainbow Brackets:智能匹配括号，
		<bold> 可以实现配对括号相同颜色,并且实现选中区域代码高亮的功能。 对增强写代码的有趣性和排错</bold>
	</li>
	<li>
		W3C Validators :adds ability to validate css and html pages with well-known W3C validation services
	</li>
	<li>
		<b>Grazie</b>
		Provides intelligent spelling and grammar checks for text that you write in the IDE.
		Supports over 15 languages, including English, German, Russian, Chinese, and others.
		Recognizes natural language constructs in programming languages (Kotlin, Python, Java and others), markup languages (Latex, Markdown, XML, HTML), comments, commits messages, and more.
		Uses LanguageTool as its proofreading engine.
		English is enabled by default when you install the plugin. To enable other languages, open Settings/Preferences and select Tools > Grazie.
		NOTE: All verification is performed locally, inside your IDE.
		Change Notes
		2019.1-4
		Fixes for UI
		Bug fixes for Markdown
		Performance improvement of XM
	</li>
</ul>

## The current inotify(7) watch limit is too low. 

1. Add the following line to either /etc/sysctl.conf file or a new *.conf file (e.g. idea.conf) under /etc/sysctl.d/ directory:
fs.inotify.max_user_watches = 524288  (recomend /etc/sysctl.d/idea.conf )

2. Then run this command to apply the change:
sudo sysctl -p --system

And don't forget to restart your IDE.