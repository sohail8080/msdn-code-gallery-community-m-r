# MetroTips #18 Webカメラで撮影するには？
## Requires
- Visual Studio 2012
## License
- MS-LPL
## Technologies
- Windows 8
- Windows RT
- Windows Store app
## Topics
- Audio and video
- web camera
- CameraCaptureUI
- CaptureElement
- MediaCapture
## Updated
- 12/20/2012
## Description

<div>これは次の記事のサンプルコードです。</div>
<blockquote>
<div>@IT 2012/12/20 掲載<br>
<img id="68637" src="http://i1.code.msdn.s-msft.com/windowsapps/metrotips-10-cb520e60/image/file/68637/1/gyoumu_newallarticle_icon_61_1349480145.gif" alt="" width="80" height="60"><br>
<strong><a href="http://www.atmarkit.co.jp/ait/articles/1212/20/news068.html" target="_blank">WinRT／Metro TIPS： Webカメラで撮影するには？［Win 8］</a></strong><br>
タブレットPCやノートPCの多くには、「Webカメラ」と呼ばれるカメラが搭載されている。これを使ってWindowsストア・アプリで写真や動画を撮影するには、どうしたらよいだろうか？ 本稿では、Webカメラで写真を撮る方法を説明する。</div>
</blockquote>
<div>以下の解説は記事からの抜粋である。かなり端折ってあるので、ぜひ記事のほうもお読みいただきたい。</div>
<div>&nbsp;</div>
<h1>Building the Sample</h1>
<div>Windows 8 製品版(または評価版) &#43; Visual Studio 2012 製品版(Expressでも可) でビルドしてほしい。<br>
これらを準備するには、<a href="http://www.atmarkit.co.jp/ait/articles/1208/23/news131.html" target="_blank">第1回のTIPS</a>を参考にしてほしい。本稿では64bit版Win 8 Proと<a href="http://www.microsoft.com/visualstudio/jpn/downloads#d-2012-express" target="_blank">VS 2012 Express for Windows
 8</a>を使用している。</div>
<div>&nbsp;</div>
<div><img id="73014" src="73014-metrotips018_04.png" alt="" width="480" height="270"></div>
<div>&nbsp;</div>
<h1>Description</h1>
<h2>●簡単に写真を撮るには？</h2>
<div>CameraCaptureUIクラス（Windows.Media.Capture名前空間）を使うと、簡単に写真や動画を撮影できる。撮影用のUI（ユーザー・インターフェイス）が用意されているので、次のようなコードを書くだけでよい。</div>
<div>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>Visual Basic</span><span>C#</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">スクリプトの編集</span>|<span class="pluginRemoveHolderLink">Remove</span></div>
<span class="hidden">vb</span><span class="hidden">csharp</span>


<div class="preview">
<pre class="vb"><span class="visualBasic__com">'&nbsp;ダイアログ生成</span>&nbsp;
<span class="visualBasic__keyword">Dim</span>&nbsp;dialog&nbsp;=&nbsp;<span class="visualBasic__keyword">New</span>&nbsp;Windows.Media.Capture.CameraCaptureUI()&nbsp;
&nbsp;
<span class="visualBasic__com">'&nbsp;写真のフォーマットを指定</span>&nbsp;
dialog.PhotoSettings.Format&nbsp;_&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=&nbsp;Windows.Media.Capture.CameraCaptureUIPhotoFormat.Png&nbsp;
&nbsp;
<span class="visualBasic__com">'&nbsp;写真撮影モードでダイアログを起動</span>&nbsp;
<span class="visualBasic__keyword">Dim</span>&nbsp;file&nbsp;=&nbsp;Await&nbsp;dialog.CaptureFileAsync(&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Windows.Media.Capture.CameraCaptureUIMode.Photo)&nbsp;
&nbsp;
<span class="visualBasic__com">'&nbsp;撮影できると、保存されたファイルのStorageFileが返ってくる。</span>&nbsp;
<span class="visualBasic__com">'&nbsp;保存先はApplicationData.Current.TemporaryFolder固定</span>&nbsp;
<span class="visualBasic__keyword">If</span>&nbsp;(file&nbsp;<span class="visualBasic__keyword">IsNot</span>&nbsp;<span class="visualBasic__keyword">Nothing</span>)&nbsp;<span class="visualBasic__keyword">Then</span>&nbsp;
&nbsp;&nbsp;<span class="visualBasic__keyword">Dim</span>&nbsp;msgbox&nbsp;=&nbsp;<span class="visualBasic__keyword">New</span>&nbsp;Windows.UI.Popups.MessageDialog(file.Path,&nbsp;<span class="visualBasic__string">&quot;撮影結果&quot;</span>)&nbsp;
&nbsp;&nbsp;Await&nbsp;msgbox.ShowAsync()&nbsp;
<span class="visualBasic__keyword">End</span>&nbsp;<span class="visualBasic__keyword">If</span></pre>
</div>
</div>
</div>
<div class="endscriptcode">&nbsp;</div>
</div>
<p>&nbsp;</p>
<h2>●独自のUIを構築するには？</h2>
<p>CameraCaptureUIクラスが提供するUIはお仕着せのものだった。CaptureElementコントロール（Windows.UI.Xaml.Controls名前空間）と、MediaCaptureクラス（Windows.Media.Capture名前空間）を使えば、独自のUIを作ることもできる。</p>
<p>こんなUIを作ってみよう。画面の左半分にCaptureElementコントロールを置き、プレビューを表示する。右半分にはImageコントロールを置いて、撮影した画像を表示する。また、下部にはボタンを3つ配置し、左の2つはプレビュー開始、右端は撮影とした。 (完成した画面が上に掲載した画像)</p>
<p>この部分のXAMLコードは次のようになる。</p>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>XAML</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">スクリプトの編集</span>|<span class="pluginRemoveHolderLink">Remove</span></div>
<span class="hidden">xaml</span>

<div class="preview">
<pre class="xaml"><span class="xaml__tag_start">&lt;Grid</span>&nbsp;Grid.<span class="xaml__attr_name">Row</span>=<span class="xaml__attr_value">&quot;1&quot;</span>&nbsp;<span class="xaml__attr_name">Background</span>=<span class="xaml__attr_value">&quot;#002244&quot;</span><span class="xaml__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;<span class="xaml__tag_start">&lt;Grid</span>.RowDefinitions<span class="xaml__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;<span class="xaml__tag_start">&lt;RowDefinition</span>&nbsp;<span class="xaml__attr_name">Height</span>=<span class="xaml__attr_value">&quot;*&quot;</span>&nbsp;<span class="xaml__tag_start">/&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="xaml__tag_start">&lt;RowDefinition</span>&nbsp;<span class="xaml__attr_name">Height</span>=<span class="xaml__attr_value">&quot;Auto&quot;</span>&nbsp;<span class="xaml__tag_start">/&gt;</span>&nbsp;
&nbsp;&nbsp;&lt;/Grid.RowDefinitions&gt;&nbsp;
&nbsp;
&nbsp;&nbsp;<span class="xaml__tag_start">&lt;Grid</span><span class="xaml__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;<span class="xaml__tag_start">&lt;Grid</span>.ColumnDefinitions<span class="xaml__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="xaml__tag_start">&lt;ColumnDefinition</span>&nbsp;<span class="xaml__attr_name">Width</span>=<span class="xaml__attr_value">&quot;1*&quot;</span>&nbsp;<span class="xaml__tag_start">/&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="xaml__tag_start">&lt;ColumnDefinition</span>&nbsp;<span class="xaml__attr_name">Width</span>=<span class="xaml__attr_value">&quot;1*&quot;</span>&nbsp;<span class="xaml__tag_start">/&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&lt;/Grid.ColumnDefinitions&gt;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="xaml__tag_start">&lt;CaptureElement</span>&nbsp;x:<span class="xaml__attr_name">Name</span>=<span class="xaml__attr_value">&quot;capturePreview&quot;</span>&nbsp;Grid.<span class="xaml__attr_name">Column</span>=<span class="xaml__attr_value">&quot;0&quot;</span>&nbsp;<span class="xaml__tag_start">/&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="xaml__tag_start">&lt;Image</span>&nbsp;x:<span class="xaml__attr_name">Name</span>=<span class="xaml__attr_value">&quot;captureImage&quot;</span>&nbsp;Grid.<span class="xaml__attr_name">Column</span>=<span class="xaml__attr_value">&quot;1&quot;</span>&nbsp;<span class="xaml__tag_start">/&gt;</span>&nbsp;
&nbsp;&nbsp;<span class="xaml__tag_end">&lt;/Grid&gt;</span>&nbsp;
&nbsp;
&nbsp;&nbsp;<span class="xaml__tag_start">&lt;StackPanel</span>&nbsp;Grid.<span class="xaml__attr_name">Row</span>=<span class="xaml__attr_value">&quot;1&quot;</span>&nbsp;<span class="xaml__attr_name">Orientation</span>=<span class="xaml__attr_value">&quot;Horizontal&quot;</span>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="xaml__attr_name">HorizontalAlignment</span>=<span class="xaml__attr_value">&quot;Center&quot;</span>&nbsp;<span class="xaml__attr_name">Margin</span>=<span class="xaml__attr_value">&quot;0,10&quot;</span><span class="xaml__tag_start">&gt;&nbsp;
</span>&nbsp;&nbsp;&nbsp;&nbsp;<span class="xaml__tag_start">&lt;Button</span>&nbsp;&nbsp;<span class="xaml__attr_name">Content</span>=<span class="xaml__attr_value">&quot;補正無し&quot;</span>&nbsp;<span class="xaml__attr_name">HorizontalAlignment</span>=<span class="xaml__attr_value">&quot;Center&quot;</span>&nbsp;<span class="xaml__attr_name">FontSize</span>=<span class="xaml__attr_value">&quot;40&quot;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="xaml__attr_name">Click</span>=<span class="xaml__attr_value">&quot;buttonPreviewClick&quot;</span>&nbsp;<span class="xaml__tag_start">/&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="xaml__tag_start">&lt;Button</span>&nbsp;&nbsp;<span class="xaml__attr_name">Content</span>=<span class="xaml__attr_value">&quot;手ブレ補正&quot;</span>&nbsp;<span class="xaml__attr_name">HorizontalAlignment</span>=<span class="xaml__attr_value">&quot;Center&quot;</span>&nbsp;<span class="xaml__attr_name">FontSize</span>=<span class="xaml__attr_value">&quot;40&quot;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="xaml__attr_name">Click</span>=<span class="xaml__attr_value">&quot;buttonPreviewWithVideoStabilizationClick&quot;</span>&nbsp;<span class="xaml__tag_start">/&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="xaml__tag_start">&lt;Button</span>&nbsp;&nbsp;<span class="xaml__attr_name">Content</span>=<span class="xaml__attr_value">&quot;《&nbsp;撮影&nbsp;》&quot;</span>&nbsp;<span class="xaml__attr_name">HorizontalAlignment</span>=<span class="xaml__attr_value">&quot;Center&quot;</span>&nbsp;<span class="xaml__attr_name">FontSize</span>=<span class="xaml__attr_value">&quot;40&quot;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="xaml__attr_name">Click</span>=<span class="xaml__attr_value">&quot;buttonTakePhotoClick&quot;</span>&nbsp;<span class="xaml__tag_start">/&gt;</span>&nbsp;
&nbsp;&nbsp;<span class="xaml__tag_end">&lt;/StackPanel&gt;</span>&nbsp;
&nbsp;
<span class="xaml__tag_end">&lt;/Grid&gt;</span></pre>
</div>
</div>
</div>
<div class="endscriptcode">&nbsp;</div>
<p>&nbsp;</p>
<h2>●プレビューを表示するには？</h2>
<p>次のコードで、Webカメラの画像がCaptureElementコントロールに映し出されるようになる。</p>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>Visual Basic</span><span>C#</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">スクリプトの編集</span>|<span class="pluginRemoveHolderLink">Remove</span></div>
<span class="hidden">vb</span><span class="hidden">csharp</span>


<div class="preview">
<pre class="vb"><span class="visualBasic__keyword">Private</span>&nbsp;Async&nbsp;<span class="visualBasic__keyword">Sub</span>&nbsp;buttonPreviewClick(sender&nbsp;<span class="visualBasic__keyword">As</span>&nbsp;<span class="visualBasic__keyword">Object</span>,&nbsp;e&nbsp;<span class="visualBasic__keyword">As</span>&nbsp;RoutedEventArgs)&nbsp;
&nbsp;&nbsp;<span class="visualBasic__com">'&nbsp;静止画撮影、動画録画を行うキャプチャ・オブジェクト</span>&nbsp;
&nbsp;&nbsp;<span class="visualBasic__keyword">Dim</span>&nbsp;_capture&nbsp;=&nbsp;<span class="visualBasic__keyword">New</span>&nbsp;Windows.Media.Capture.MediaCapture()&nbsp;
&nbsp;
&nbsp;&nbsp;<span class="visualBasic__keyword">Try</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="visualBasic__com">'&nbsp;Webカメラの初期化</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;Await&nbsp;_capture.InitializeAsync()&nbsp;
&nbsp;
&nbsp;&nbsp;<span class="visualBasic__keyword">Catch</span>&nbsp;ex&nbsp;<span class="visualBasic__keyword">As</span>&nbsp;UnauthorizedAccessException&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="visualBasic__com">'&nbsp;Webカメラがユーザーに許可されていないと、</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="visualBasic__com">'&nbsp;UnauthorizedAccessExceptionが出る</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="visualBasic__keyword">Return</span>&nbsp;
&nbsp;&nbsp;<span class="visualBasic__keyword">End</span>&nbsp;<span class="visualBasic__keyword">Try</span>&nbsp;
&nbsp;
&nbsp;&nbsp;<span class="visualBasic__com">'&nbsp;Effectの追加はここで行う&nbsp;&hellip;&hellip;&nbsp;（1）</span>&nbsp;
&nbsp;
&nbsp;&nbsp;<span class="visualBasic__com">'&nbsp;MediaCaptureインスタンスをCaptureElementコントロールに設定する</span>&nbsp;
&nbsp;&nbsp;capturePreview.Source&nbsp;=&nbsp;_capture&nbsp;
&nbsp;
&nbsp;&nbsp;<span class="visualBasic__com">'&nbsp;プレビューを開始する</span>&nbsp;
&nbsp;&nbsp;Await&nbsp;_capture.StartPreviewAsync()&nbsp;
<span class="visualBasic__keyword">End</span>&nbsp;<span class="visualBasic__keyword">Sub</span></pre>
</div>
</div>
</div>
<div class="endscriptcode">&nbsp;</div>
<p>&nbsp;</p>
<h2>●プレビューした画像を撮影するには？</h2>
<p>プレビューに表示されている映像から写真を撮影するには、次のようなコードを記述する。</p>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>Visual Basic</span><span>C#</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">スクリプトの編集</span>|<span class="pluginRemoveHolderLink">Remove</span></div>
<span class="hidden">vb</span><span class="hidden">csharp</span>


<div class="preview">
<pre class="vb"><span class="visualBasic__keyword">Private</span>&nbsp;Async&nbsp;<span class="visualBasic__keyword">Sub</span>&nbsp;buttonTakePhotoClick(sender&nbsp;<span class="visualBasic__keyword">As</span>&nbsp;<span class="visualBasic__keyword">Object</span>,&nbsp;e&nbsp;<span class="visualBasic__keyword">As</span>&nbsp;RoutedEventArgs)&nbsp;
&nbsp;&nbsp;<span class="visualBasic__keyword">If</span>&nbsp;(capturePreview.Source&nbsp;<span class="visualBasic__keyword">Is</span>&nbsp;<span class="visualBasic__keyword">Nothing</span>)&nbsp;<span class="visualBasic__keyword">Then</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="visualBasic__keyword">Return</span>&nbsp;
&nbsp;&nbsp;<span class="visualBasic__keyword">End</span>&nbsp;<span class="visualBasic__keyword">If</span>&nbsp;
&nbsp;
&nbsp;&nbsp;<span class="visualBasic__com">'&nbsp;プレビューに付けたMediaCaptureオブジェクトを取り出す</span>&nbsp;
&nbsp;&nbsp;<span class="visualBasic__keyword">Dim</span>&nbsp;capture&nbsp;<span class="visualBasic__keyword">As</span>&nbsp;Windows.Media.Capture.MediaCapture&nbsp;=&nbsp;capturePreview.Source&nbsp;
&nbsp;
&nbsp;&nbsp;<span class="visualBasic__com">'&nbsp;保存する画像ファイルのフォーマット（.pngファイルを指定）</span>&nbsp;
&nbsp;&nbsp;<span class="visualBasic__keyword">Dim</span>&nbsp;imageProperties&nbsp;=&nbsp;Windows.Media.MediaProperties.ImageEncodingProperties.CreatePng()&nbsp;
&nbsp;
&nbsp;&nbsp;<span class="visualBasic__com">'&nbsp;撮影する。一度ファイルに保存する</span>&nbsp;
&nbsp;&nbsp;<span class="visualBasic__keyword">Dim</span>&nbsp;file&nbsp;<span class="visualBasic__keyword">As</span>&nbsp;StorageFile&nbsp;=&nbsp;Await&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;ApplicationData.Current.TemporaryFolder.CreateFileAsync(<span class="visualBasic__string">&quot;test.png&quot;</span>,&nbsp;&nbsp;&nbsp;&nbsp;CreationCollisionOption.GenerateUniqueName)&nbsp;
&nbsp;&nbsp;<span class="visualBasic__keyword">Try</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;Await&nbsp;capture.CapturePhotoToStorageFileAsync(imageProperties,&nbsp;file)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="visualBasic__com">'&nbsp;キャプチャ開始から撮影までの間に許可を取り消されると、ここで例外が出る</span>&nbsp;
&nbsp;&nbsp;<span class="visualBasic__keyword">Catch</span>&nbsp;ex&nbsp;<span class="visualBasic__keyword">As</span>&nbsp;Exception&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="visualBasic__keyword">Return</span>&nbsp;
&nbsp;&nbsp;<span class="visualBasic__keyword">End</span>&nbsp;<span class="visualBasic__keyword">Try</span>&nbsp;
&nbsp;
&nbsp;&nbsp;<span class="visualBasic__com">'撮影したファイルを読み込んで右側のImageコントロールに表示する</span>&nbsp;
&nbsp;&nbsp;<span class="visualBasic__keyword">Using</span>&nbsp;stream&nbsp;=&nbsp;Await&nbsp;file.OpenReadAsync()&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="visualBasic__keyword">Dim</span>&nbsp;bi&nbsp;=&nbsp;<span class="visualBasic__keyword">New</span>&nbsp;BitmapImage()&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;bi.SetSource(stream)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;captureImage.Source&nbsp;=&nbsp;bi&nbsp;
&nbsp;&nbsp;<span class="visualBasic__keyword">End</span>&nbsp;<span class="visualBasic__keyword">Using</span>&nbsp;
<span class="visualBasic__keyword">End</span>&nbsp;<span class="visualBasic__keyword">Sub</span></pre>
</div>
</div>
</div>
<div class="endscriptcode">&nbsp;</div>
<p>&nbsp;</p>
<h1><span>Source Code Files</span></h1>
<ul>
<li><em>MainPage.xaml - CaptureElement コントロールを使ったプレビューと、 Image コントロールによる撮影結果を表示する画面</em>
</li><li><em>MainPage.xaml.cs/.vb - CameraCaptureUI クラスとMediaCaptureクラスによる写真撮影</em> </li></ul>
<h1>More Information</h1>
<h2>著作権について</h2>
<div>このソースコードは MS-LPL ライセンスで提供しておりますので、 ご自由に利用いただけます。<br>
ただし、@ITの記事(ここへ転載・引用した部分も含む)そのものの著作権は、筆者とデジタルアドバンテージが保有しており、サンプルコード部分を除く記事の無断使用は固くお断りいたします。</div>
