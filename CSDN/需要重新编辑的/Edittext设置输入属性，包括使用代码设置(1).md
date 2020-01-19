edittext的输入属性类型
---------------

android:inputType=”none”–输入普通字符 
android:inputType=”text”–输入普通字符 
android:inputType=”textCapCharacters”–输入普通字符 
android:inputType=”textCapWords”–单词首字母大小 
android:inputType=”textCapSentences”–仅第一个字母大小 
android:inputType=”textAutoCorrect”–前两个自动完成 
android:inputType=”textAutoComplete”–前两个自动完成 
android:inputType=”textMultiLine”–多行输入 
android:inputType=”textImeMultiLine”–输入法多行（不一定支持） 
android:inputType=”textNoSuggestions”–不提示 
android:inputType=”textUri”–URI格式 
android:inputType=”textEmailAddress”–电子邮件地址格式 
android:inputType=”textEmailSubject”–邮件主题格式 
android:inputType=”textShortMessage”–短消息格式 
android:inputType=”textLongMessage”–长消息格式 
android:inputType=”textPersonName”–人名格式 
android:inputType=”textPostalAddress”–邮政格式 
android:inputType=”textPassword”–密码格式 
android:inputType=”textVisiblePassword”–密码可见格式 
android:inputType=”textWebEditText”–作为网页表单的文本格式 
android:inputType=”textFilter”–文本筛选格式 
android:inputType=”textPhonetic”–拼音输入格式 
android:inputType=”number”–数字格式 
android:inputType=”numberSigned”–有符号数字格式 
android:inputType=”numberDecimal”–可以带小数点的浮点格式 
android:inputType=”phone”–拨号键盘 
android:inputType=”datetime” 
android:inputType=”date”–日期键盘
android:inputType=”time”–时间键盘



<h1>EditText的InputType属性对应的xml定义有哪些，以及代码中设置的InputType类型有哪些</h1>
<p>知道了设置EditText的InputType属性值，既可以通过xml中定义，也可以在代码中设置为InputType的某种值，但是到底这些值有哪些，以及分别对应的含义是啥，则可以参考官网：</p>
<p><a href="http://developer.android.com/reference/android/widget/TextView.html#attr_android:inputType" target="_blank">TextView | Android Developers &#8211; android:inputType</a> </p>
<p>中的完整的列表：</p>
<table cellspacing="0" cellpadding="0" border="1">
<tbody>
<tr>
<td valign="top" width="146">
<p><b>Constant</b></p>
</td>
<td valign="top" width="95">
<p><b>Value</b></p>
</td>
<td valign="top">
<p><b>Description</b></p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>none</p>
</td>
<td valign="top" width="95">
<p>0x00000000</p>
</td>
<td valign="top">
<p>There is no content type. The text is not editable.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>text</p>
</td>
<td valign="top" width="95">
<p>0x00000001</p>
</td>
<td valign="top">
<p>Just plain old text. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_TEXT">TYPE_CLASS_TEXT</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_VARIATION_NORMAL">TYPE_TEXT_VARIATION_NORMAL</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textCapCharacters</p>
</td>
<td valign="top" width="95">
<p>0x00001001</p>
</td>
<td valign="top">
<p>Can be combined with <i>text</i> and its variations to request capitalization of all characters. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_FLAG_CAP_CHARACTERS">TYPE_TEXT_FLAG_CAP_CHARACTERS</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textCapWords</p>
</td>
<td valign="top" width="95">
<p>0x00002001</p>
</td>
<td valign="top">
<p>Can be combined with <i>text</i> and its variations to request capitalization of the first character of every word. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_FLAG_CAP_WORDS">TYPE_TEXT_FLAG_CAP_WORDS</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textCapSentences</p>
</td>
<td valign="top" width="95">
<p>0x00004001</p>
</td>
<td valign="top">
<p>Can be combined with <i>text</i> and its variations to request capitalization of the first character of every sentence. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_FLAG_CAP_SENTENCES">TYPE_TEXT_FLAG_CAP_SENTENCES</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textAutoCorrect</p>
</td>
<td valign="top" width="95">
<p>0x00008001</p>
</td>
<td valign="top">
<p>Can be combined with <i>text</i> and its variations to request auto-correction of text being input. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_FLAG_AUTO_CORRECT">TYPE_TEXT_FLAG_AUTO_CORRECT</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textAutoComplete</p>
</td>
<td valign="top" width="95">
<p>0x00010001</p>
</td>
<td valign="top">
<p>Can be combined with <i>text</i> and its variations to specify that this field will be doing its own auto-completion and talking with the input method appropriately. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_FLAG_AUTO_COMPLETE">TYPE_TEXT_FLAG_AUTO_COMPLETE</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textMultiLine</p>
</td>
<td valign="top" width="95">
<p>0x00020001</p>
</td>
<td valign="top">
<p>Can be combined with <i>text</i> and its variations to allow multiple lines of text in the field. If this flag is not set, the text field will be constrained to a single line. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_FLAG_MULTI_LINE">TYPE_TEXT_FLAG_MULTI_LINE</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textImeMultiLine</p>
</td>
<td valign="top" width="95">
<p>0x00040001</p>
</td>
<td valign="top">
<p>Can be combined with <i>text</i> and its variations to indicate that though the regular text view should not be multiple lines, the IME should provide multiple lines if it can. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_FLAG_IME_MULTI_LINE">TYPE_TEXT_FLAG_IME_MULTI_LINE</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textNoSuggestions</p>
</td>
<td valign="top" width="95">
<p>0x00080001</p>
</td>
<td valign="top">
<p>Can be combined with <i>text</i> and its variations to indicate that the IME should not show any dictionary-based word suggestions. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_FLAG_NO_SUGGESTIONS">TYPE_TEXT_FLAG_NO_SUGGESTIONS</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textUri</p>
</td>
<td valign="top" width="95">
<p>0x00000011</p>
</td>
<td valign="top">
<p>Text that will be used as a URI. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_TEXT">TYPE_CLASS_TEXT</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_VARIATION_URI">TYPE_TEXT_VARIATION_URI</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textEmailAddress</p>
</td>
<td valign="top" width="95">
<p>0x00000021</p>
</td>
<td valign="top">
<p>Text that will be used as an e-mail address. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_TEXT">TYPE_CLASS_TEXT</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_VARIATION_EMAIL_ADDRESS">TYPE_TEXT_VARIATION_EMAIL_ADDRESS</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textEmailSubject</p>
</td>
<td valign="top" width="95">
<p>0x00000031</p>
</td>
<td valign="top">
<p>Text that is being supplied as the subject of an e-mail. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_TEXT">TYPE_CLASS_TEXT</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_VARIATION_EMAIL_SUBJECT">TYPE_TEXT_VARIATION_EMAIL_SUBJECT</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textShortMessage</p>
</td>
<td valign="top" width="95">
<p>0x00000041</p>
</td>
<td valign="top">
<p>Text that is the content of a short message. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_TEXT">TYPE_CLASS_TEXT</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_VARIATION_SHORT_MESSAGE">TYPE_TEXT_VARIATION_SHORT_MESSAGE</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textLongMessage</p>
</td>
<td valign="top" width="95">
<p>0x00000051</p>
</td>
<td valign="top">
<p>Text that is the content of a long message. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_TEXT">TYPE_CLASS_TEXT</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_VARIATION_LONG_MESSAGE">TYPE_TEXT_VARIATION_LONG_MESSAGE</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textPersonName</p>
</td>
<td valign="top" width="95">
<p>0x00000061</p>
</td>
<td valign="top">
<p>Text that is the name of a person. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_TEXT">TYPE_CLASS_TEXT</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_VARIATION_PERSON_NAME">TYPE_TEXT_VARIATION_PERSON_NAME</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textPostalAddress</p>
</td>
<td valign="top" width="95">
<p>0x00000071</p>
</td>
<td valign="top">
<p>Text that is being supplied as a postal mailing address. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_TEXT">TYPE_CLASS_TEXT</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_VARIATION_POSTAL_ADDRESS">TYPE_TEXT_VARIATION_POSTAL_ADDRESS</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textPassword</p>
</td>
<td valign="top" width="95">
<p>0x00000081</p>
</td>
<td valign="top">
<p>Text that is a password. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_TEXT">TYPE_CLASS_TEXT</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_VARIATION_PASSWORD">TYPE_TEXT_VARIATION_PASSWORD</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textVisiblePassword</p>
</td>
<td valign="top" width="95">
<p>0x00000091</p>
</td>
<td valign="top">
<p>Text that is a password that should be visible. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_TEXT">TYPE_CLASS_TEXT</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_VARIATION_VISIBLE_PASSWORD">TYPE_TEXT_VARIATION_VISIBLE_PASSWORD</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textWebEditText</p>
</td>
<td valign="top" width="95">
<p>0x000000a1</p>
</td>
<td valign="top">
<p>Text that is being supplied as text in a web form. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_TEXT">TYPE_CLASS_TEXT</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_VARIATION_WEB_EDIT_TEXT">TYPE_TEXT_VARIATION_WEB_EDIT_TEXT</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textFilter</p>
</td>
<td valign="top" width="95">
<p>0x000000b1</p>
</td>
<td valign="top">
<p>Text that is filtering some other data. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_TEXT">TYPE_CLASS_TEXT</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_VARIATION_FILTER">TYPE_TEXT_VARIATION_FILTER</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textPhonetic</p>
</td>
<td valign="top" width="95">
<p>0x000000c1</p>
</td>
<td valign="top">
<p>Text that is for phonetic pronunciation, such as a phonetic name field in a contact entry. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_TEXT">TYPE_CLASS_TEXT</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_VARIATION_PHONETIC">TYPE_TEXT_VARIATION_PHONETIC</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textWebEmailAddress</p>
</td>
<td valign="top" width="95">
<p>0x000000d1</p>
</td>
<td valign="top">
<p>Text that will be used as an e-mail address on a web form. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_TEXT">TYPE_CLASS_TEXT</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_VARIATION_WEB_EMAIL_ADDRESS">TYPE_TEXT_VARIATION_WEB_EMAIL_ADDRESS</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>textWebPassword</p>
</td>
<td valign="top" width="95">
<p>0x000000e1</p>
</td>
<td valign="top">
<p>Text that will be used as a password on a web form. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_TEXT">TYPE_CLASS_TEXT</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_VARIATION_WEB_PASSWORD">TYPE_TEXT_VARIATION_WEB_PASSWORD</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>number</p>
</td>
<td valign="top" width="95">
<p>0x00000002</p>
</td>
<td valign="top">
<p>A numeric only field. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_NUMBER">TYPE_CLASS_NUMBER</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_NUMBER_VARIATION_NORMAL">TYPE_NUMBER_VARIATION_NORMAL</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>numberSigned</p>
</td>
<td valign="top" width="95">
<p>0x00001002</p>
</td>
<td valign="top">
<p>Can be combined with <i>number</i> and its other options to allow a signed number. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_NUMBER">TYPE_CLASS_NUMBER</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_NUMBER_FLAG_SIGNED">TYPE_NUMBER_FLAG_SIGNED</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>numberDecimal</p>
</td>
<td valign="top" width="95">
<p>0x00002002</p>
</td>
<td valign="top">
<p>Can be combined with <i>number</i> and its other options to allow a decimal (fractional) number. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_NUMBER">TYPE_CLASS_NUMBER</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_NUMBER_FLAG_DECIMAL">TYPE_NUMBER_FLAG_DECIMAL</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>numberPassword</p>
</td>
<td valign="top" width="95">
<p>0x00000012</p>
</td>
<td valign="top">
<p>A numeric password field. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_NUMBER">TYPE_CLASS_NUMBER</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_NUMBER_VARIATION_PASSWORD">TYPE_NUMBER_VARIATION_PASSWORD</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>phone</p>
</td>
<td valign="top" width="95">
<p>0x00000003</p>
</td>
<td valign="top">
<p>For entering a phone number. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_PHONE">TYPE_CLASS_PHONE</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>datetime</p>
</td>
<td valign="top" width="95">
<p>0x00000004</p>
</td>
<td valign="top">
<p>For entering a date and time. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_DATETIME">TYPE_CLASS_DATETIME</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_DATETIME_VARIATION_NORMAL">TYPE_DATETIME_VARIATION_NORMAL</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>date</p>
</td>
<td valign="top" width="95">
<p>0x00000014</p>
</td>
<td valign="top">
<p>For entering a date. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_DATETIME">TYPE_CLASS_DATETIME</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_DATETIME_VARIATION_DATE">TYPE_DATETIME_VARIATION_DATE</a>.</p>
</td>
</tr>
<tr>
<td valign="top" width="146">
<p>time</p>
</td>
<td valign="top" width="95">
<p>0x00000024</p>
</td>
<td valign="top">
<p>For entering a time. Corresponds to <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_CLASS_DATETIME">TYPE_CLASS_DATETIME</a> | <a href="http://developer.android.com/reference/android/text/InputType.html#TYPE_DATETIME_VARIATION_TIME">TYPE_DATETIME_VARIATION_TIME</a>.</p>
</td>
</tr>
</tbody>
</table>
<p>&#160;</p>
<p>如此，就可以自己去在xml或代码中，分别试试，每种不同的InputType对应的都是什么效果了。</p>
<p>&#160;</p>
<h1>注意：通过代码给InputType赋值时，不是设置TYPE_XXX_VARIATION_YYY，而是要设置TYPE_CLASS_XXX | TYPE_XXXX_VARAITION_YYY</h1>
<p>之前在代码中给InputType设置值，错写成：</p>
<pre class="brush: java; auto-links: true; collapse: false; first-line: 1; gutter: true; html-script: false; light: false; ruler: false; smart-tabs: true; tab-size: 4; toolbar: true;">inputType = InputType.TYPE_DATETIME_VARIATION_TIME;</pre>
<p>导致，EditText点击后，不显示输入法键盘，改为正确的：</p>
<pre class="brush: java; auto-links: true; collapse: false; first-line: 1; gutter: true; html-script: false; light: false; ruler: false; smart-tabs: true; tab-size: 4; toolbar: true;">inputType = InputType.TYPE_CLASS_DATETIME | InputType.TYPE_DATETIME_VARIATION_TIME;</pre>
<p>就可以正常的显示键盘了。 </p>
<p>&#160;</p>


使用代码设置：
```
    edtInput.setInputType(InputType.TYPE_CLASS_NUMBER | InputType.TYPE_NUMBER_FLAG_DECIMAL);
```


以上部分内容摘自：http://www.crifan.com/summary_android_edittext_inputtype_values_and_meaning_definition/