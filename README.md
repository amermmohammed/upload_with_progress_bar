# upload_with_progress_bar
<p dir="rtl">ألق نظرة على بنية الملف.</p>
<pre><code class="hljs">php_file_upload_with_progress_bar
├── index.html
├── upload.php
├── uploads/
├── js/
│   └── jquery.min.js
├── css/
│   └── style.css
└── images/</code></pre>
<h2 dir="rtl">نموذج تحميل الملف مع شريط التقدم</h2>
<p dir="rtl">يعالج ملف index.html اختيار الملف وعمليات عرض تقدم التحميل المباشر.</p>
<p dir="rtl"><b>نموذج رفع الملف:</b><br>
<b>1.</b>&nbsp;قم بإنشاء نموذج HTML مع حقل إدخال ملف وزر إرسال.
</p>
<ul dir="rtl">
<li>يجب أن تحتوي &nbsp;&lt;form&gt;&nbsp;على&nbsp;<code>enctype="multipart/form-data"</code>&nbsp;</li>
<li>يجب أن تحتوي  &nbsp;&lt;input&gt;&nbsp; على &nbsp;<code>type="file"</code>.</li>
</ul>
<pre><code class="language-html hljs xml"><span class="hljs-comment">&lt;!-- File upload form --&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">form</span> <span class="hljs-attr">id</span>=<span class="hljs-string">"uploadForm"</span> <span class="hljs-attr">enctype</span>=<span class="hljs-string">"multipart/form-data"</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">label</span>&gt;</span>Choose File:<span class="hljs-tag">&lt;/<span class="hljs-name">label</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">input</span> <span class="hljs-attr">type</span>=<span class="hljs-string">"file"</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"file"</span> <span class="hljs-attr">id</span>=<span class="hljs-string">"fileInput"</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">input</span> <span class="hljs-attr">type</span>=<span class="hljs-string">"submit"</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"submit"</span> <span class="hljs-attr">value</span>=<span class="hljs-string">"UPLOAD"</span>/&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">form</span>&gt;</span></code></pre>

<p dir="rtl"><b>2.</b>&nbsp;حدد عنصر HTML لعرض شريط التقدم.</p>

<pre><code class="language-html hljs xml"><span class="hljs-comment">&lt;!-- Progress bar --&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">div</span> <span class="hljs-attr">class</span>=<span class="hljs-string">"progress"</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">div</span> <span class="hljs-attr">class</span>=<span class="hljs-string">"progress-bar"</span>&gt;</span><span class="hljs-tag">&lt;/<span class="hljs-name">div</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">div</span>&gt;</span></code></pre>

<p dir="rtl"><b>3.</b>&nbsp;حدد عنصر HTML لعرض حالة تحميل الملف.</p>

<pre><code class="language-html hljs xml"><span class="hljs-comment">&lt;!-- Display upload status --&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">div</span> <span class="hljs-attr">id</span>=<span class="hljs-string">"uploadStatus"</span>&gt;</span><span class="hljs-tag">&lt;/<span class="hljs-name">div</span>&gt;</span></code></pre>

<p dir="rtl"><b>تحميل ملف Ajax مع شريط التقدم:</b></p>

<p dir="rtl">يتم استخدام jQuery و Ajax لتحميل ملف مع شريط تقدم ، لذلك قم بتضمين مكتبة jQuery أولاً.</p>

<pre><code class="language-javascript hljs">&lt;script src=<span class="hljs-string">"https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"</span>&gt;<span class="xml"><span class="hljs-tag">&lt;/<span class="hljs-name">script</span>&gt;</span></span>
</code></pre>


<p dir="rtl">يرسل كود jQuery التالي بيانات الملف المحدد إلى البرنامج النصي من جانب الخادم دون إعادة تحميل الصفحة عبر Ajax.</p>

<ul dir="rtl">
<li>عند إرسال النموذج ، يتم إرسال بيانات الملف المحدد إلى البرنامج النصي من جانب الخادم (<code> upload.php </code>) باستخدام jQuery و Ajax.
<ul dir="rtl">
<li>يتم استخدام خيار &nbsp;<code>xhr</code>&nbsp; للطريقة &nbsp;<code>$.ajax()</code>&nbsp; لتتبع تقدم التحميل.</li>
<li>تقوم الطريقة &nbsp;<code>window.XMLHttpRequest()</code>&nbsp; بإنشاء كائن XMLHttpRequest جديد.</li>
<li>تشير الخاصية &nbsp;<code>progress</code>&nbsp; حدث XMLHttpRequest &nbsp;<code>upload</code>&nbsp; إلى مقدار التقدم الذي تم إحرازه حتى الآن.</li>
<li>نسبة تقدم التحميل مرفقة بشريط التقدم.</li>
</ul>
</li>
<li>يتم استخدام كائن FormData لاسترداد بيانات الملف المقدمة.
<ul dir="rtl">
<li>بناءً على الاستجابة ، يتم عرض حالة التحميل على صفحة الويب.</li>
</ul>
</li>
<li>عند حدث التغيير ، يتم التحقق من نوع الملف بناءً على الأنواع المسموح بها.
<ul>
<li>يتم استخدام File API للحصول على نوع الملف المحدد.</li>
<li>يتم التحقق من صحة نوع MIME للملف المحدد ويقيد المستخدم في تحميل الصورة فقط (.jpeg / .jpg / .png / .gif) أو ملف PDF (.pdf) أو ملف MS Word (.doc / .docx).</li>
<li>تحدد الطريقة &nbsp;<code>includes()</code>&nbsp; ما إذا كانت مصفوفة الأنواع المسموح بها تحتوي على نوع الملف المحدد</li>
</ul>
</li>
</ul>

<pre><code class="language-javascript hljs">&lt;script&gt;
$(<span class="hljs-built_in">document</span>).ready(<span class="hljs-function"><span class="hljs-keyword">function</span>(<span class="hljs-params"></span>)</span>{
    <span class="hljs-comment">// File upload via Ajax</span>
    $(<span class="hljs-string">"#uploadForm"</span>).on(<span class="hljs-string">'submit'</span>, <span class="hljs-function"><span class="hljs-keyword">function</span>(<span class="hljs-params">e</span>)</span>{
        e.preventDefault();
        $.ajax({
            <span class="hljs-attr">xhr</span>: <span class="hljs-function"><span class="hljs-keyword">function</span>(<span class="hljs-params"></span>) </span>{
                <span class="hljs-keyword">var</span> xhr = <span class="hljs-keyword">new</span> <span class="hljs-built_in">window</span>.XMLHttpRequest();
                xhr.upload.addEventListener(<span class="hljs-string">"progress"</span>, <span class="hljs-function"><span class="hljs-keyword">function</span>(<span class="hljs-params">evt</span>) </span>{
                    <span class="hljs-keyword">if</span> (evt.lengthComputable) {
                        <span class="hljs-keyword">var</span> percentComplete = ((evt.loaded / evt.total) * <span class="hljs-number">100</span>);
                        $(<span class="hljs-string">".progress-bar"</span>).width(percentComplete + <span class="hljs-string">'%'</span>);
                        $(<span class="hljs-string">".progress-bar"</span>).html(percentComplete+<span class="hljs-string">'%'</span>);
                    }
                }, <span class="hljs-literal">false</span>);
                <span class="hljs-keyword">return</span> xhr;
            },
            <span class="hljs-attr">type</span>: <span class="hljs-string">'POST'</span>,
            <span class="hljs-attr">url</span>: <span class="hljs-string">'upload.php'</span>,
            <span class="hljs-attr">data</span>: <span class="hljs-keyword">new</span> FormData(<span class="hljs-keyword">this</span>),
            <span class="hljs-attr">contentType</span>: <span class="hljs-literal">false</span>,
            <span class="hljs-attr">cache</span>: <span class="hljs-literal">false</span>,
            <span class="hljs-attr">processData</span>:<span class="hljs-literal">false</span>,
            <span class="hljs-attr">beforeSend</span>: <span class="hljs-function"><span class="hljs-keyword">function</span>(<span class="hljs-params"></span>)</span>{
                $(<span class="hljs-string">".progress-bar"</span>).width(<span class="hljs-string">'0%'</span>);
                $(<span class="hljs-string">'#uploadStatus'</span>).html(<span class="hljs-string">'&lt;img src="images/loading.gif"/&gt;'</span>);
            },
            <span class="hljs-attr">error</span>:<span class="hljs-function"><span class="hljs-keyword">function</span>(<span class="hljs-params"></span>)</span>{
                $(<span class="hljs-string">'#uploadStatus'</span>).html(<span class="hljs-string">'&lt;p style="color:#EA4335;"&gt;File upload failed, please try again.&lt;/p&gt;'</span>);
            },
            <span class="hljs-attr">success</span>: <span class="hljs-function"><span class="hljs-keyword">function</span>(<span class="hljs-params">resp</span>)</span>{
                <span class="hljs-keyword">if</span>(resp == <span class="hljs-string">'ok'</span>){
                    $(<span class="hljs-string">'#uploadForm'</span>)[<span class="hljs-number">0</span>].reset();
                    $(<span class="hljs-string">'#uploadStatus'</span>).html(<span class="hljs-string">'&lt;p style="color:#28A74B;"&gt;File has uploaded successfully!&lt;/p&gt;'</span>);
                }<span class="hljs-keyword">else</span> <span class="hljs-keyword">if</span>(resp == <span class="hljs-string">'err'</span>){
                    $(<span class="hljs-string">'#uploadStatus'</span>).html(<span class="hljs-string">'&lt;p style="color:#EA4335;"&gt;Please select a valid file to upload.&lt;/p&gt;'</span>);
                }
            }
        });
    });
	
    <span class="hljs-comment">// File type validation</span>
    $(<span class="hljs-string">"#fileInput"</span>).change(<span class="hljs-function"><span class="hljs-keyword">function</span>(<span class="hljs-params"></span>)</span>{
        <span class="hljs-keyword">var</span> allowedTypes = [<span class="hljs-string">'application/pdf'</span>, <span class="hljs-string">'application/msword'</span>, <span class="hljs-string">'application/vnd.ms-office'</span>, <span class="hljs-string">'application/vnd.openxmlformats-officedocument.wordprocessingml.document'</span>, <span class="hljs-string">'image/jpeg'</span>, <span class="hljs-string">'image/png'</span>, <span class="hljs-string">'image/jpg'</span>, <span class="hljs-string">'image/gif'</span>];
        <span class="hljs-keyword">var</span> file = <span class="hljs-keyword">this</span>.files[<span class="hljs-number">0</span>];
        <span class="hljs-keyword">var</span> fileType = file.type;
        <span class="hljs-keyword">if</span>(!allowedTypes.includes(fileType)){
            alert(<span class="hljs-string">'Please select a valid file (PDF/DOC/DOCX/JPEG/JPG/PNG/GIF).'</span>);
            $(<span class="hljs-string">"#fileInput"</span>).val(<span class="hljs-string">''</span>);
            <span class="hljs-keyword">return</span> <span class="hljs-literal">false</span>;
        }
    });
});
<span class="xml"><span class="hljs-tag">&lt;/<span class="hljs-name">script</span>&gt;</span></span></code></pre>

<h2 dir="rtl">تحميل الملف إلى الخادم باستخدام PHP</h2>
<p dir="rtl">يتم استدعاء الملف &nbsp;<code>upload.php</code>&nbsp; بواسطة طلب Ajax لمعالجة عملية تحميل الملف باستخدام PHP.</p>

<ul dir="rtl">
<li>استرجع معلومات الملف من البيانات المنشورة باستخدام طريقة &nbsp;<b>PHP $_FILES</b>&nbsp;</li>
<li>قم بتحميل الملف إلى الخادم باستخدام الوظيفة &nbsp;<b>()move_uploaded_file</b>&nbsp; في PHP.</li>
<li>تقديم حالة تحميل الملف التي تعود إلى وظيفة نجاح Ajax.</li>
</ul>
<pre><code class="language-php hljs"><span class="hljs-meta">&lt;?php</span> 
$upload = <span class="hljs-string">'err'</span>; 
<span class="hljs-keyword">if</span>(!<span class="hljs-keyword">empty</span>($_FILES[<span class="hljs-string">'file'</span>])){ 

    <span class="hljs-comment">// File upload configuration </span>
    $targetDir = <span class="hljs-string">"uploads/"</span>; 
    $allowTypes = <span class="hljs-keyword">array</span>(<span class="hljs-string">'pdf'</span>, <span class="hljs-string">'doc'</span>, <span class="hljs-string">'docx'</span>, <span class="hljs-string">'jpg'</span>, <span class="hljs-string">'png'</span>, <span class="hljs-string">'jpeg'</span>, <span class="hljs-string">'gif'</span>); 
     
    $fileName = basename($_FILES[<span class="hljs-string">'file'</span>][<span class="hljs-string">'name'</span>]); 
    $targetFilePath = $targetDir.$fileName; 
     
    <span class="hljs-comment">// Check whether file type is valid </span>
    $fileType = pathinfo($targetFilePath, PATHINFO_EXTENSION); 
    <span class="hljs-keyword">if</span>(in_array($fileType, $allowTypes)){ 
        <span class="hljs-comment">// Upload file to the server </span>
        <span class="hljs-keyword">if</span>(move_uploaded_file($_FILES[<span class="hljs-string">'file'</span>][<span class="hljs-string">'tmp_name'</span>], $targetFilePath)){ 
            $upload = <span class="hljs-string">'ok'</span>; 
        } 
    } 
}
<span class="hljs-keyword">echo</span> $upload;
<span class="hljs-meta">?&gt;</span></code></pre>

[Source](https://www.laravelcode.com/post/how-to-files-upload-with-progress-bar-help-of-jquery-ajax-php)