require 'json'
require 'nokogiri'

task :default => [:default_task]

task :default_task do
  puts "Hello, world"
end

task :add_link_to_translator do
  content = open('target/_layouts/default.html', 'r').read.gsub(/{{ content }}/, "{{content}} <div style='margin-top: 4em; font-size: 16px;'>이 사이트는 <a href='nacyot.com'>nacyot</a>이 번역했으며, <a href='http://github.com/nacyot/cask.emacs.kr'>Github 저장소</a>에서 관리되고 있습니다.</div>")
  File.open('target/_layouts/default.html', 'w+'){ |f| f.write content }
end

task :rewrite_cname do
  File.open('target/CNAME', 'w+'){ |f| f.write "cask.emacs.kr" }
end

task :resize_font do
  content = open('target/css/cask.css', 'r').read.gsub(/body {/, "body { font-size: 2em;\n")
  File.open('target/css/cask.css', 'w+'){|f| f.write content}
end

task :rebuild_sidebar do
  sidebar = <<Sidebar
                <li>
                  <a href="/installation.html">설치</a>
                </li>
                <li>
                  <a href="/examples.html">예제</a>
                </li>
                <li>
                  <a href="/license.html">라이센스</a>
                </li>
                <li>
                  <a href="/api.html">API</a>
                </li>
                <li>
                  <a href="/why-cask.html">왜 Cask인가?</a>
                </li>
                <li class="active">
                  <a href="/troubleshoot.html">문제 해결하기</a>
                </li>
                <li>
                  <a href="/dsl.html">DSL</a>
                </li>
                <li>
                  <a href="/development.html">개발</a>
                </li>
                <li>
                  <a href="/usage.html">사용법</a>
                </li>
                <li>
                  <a href="/tools.html">도구</a>
                </li>
                <li>
                  <a href="/index.html">Cask</a>
                </li>
Sidebar

  content = open('target/_layouts/default.html', 'r').read.gsub(/{% for.*?endfor %}/m, sidebar)
  File.open('target/_layouts/default.html', 'w+'){ |f| f.write content }
end

task :add_links do
  links_html = JSON.parse(open('data/sites.json').read)
    .delete_if{|link| link["language"] != "kr" }
    .map{|link| "<li><a href='#{link["url"]}'>#{link["title"]}</a></li>\n"}
    .inject(""){|text, link| text += link}
  
  doc = Nokogiri::HTML(open('target/index.html'))
  target = doc.search("#be-free").first

  node = Nokogiri::XML::Node.new("section", doc)
  node["class"] = "white"
  node.inner_html = "<div>\n<h2>한국어 자료</h2>\n<p>\n<ul>\n" + links_html + "</ul>\n</p>\n</div>"

  target.add_previous_sibling node
  
  File.open('target/index.html', 'w+'){ |f| doc.write_xml_to f }
end
