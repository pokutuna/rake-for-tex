require 'rake/clean'
require 'yaml'

config = YAML.load_file(File.dirname(__FILE__) + '/config.yaml')
target = config['target']

img_require_bb = FileList['img/*.png', 'img/*.jpg', 'img/*.pdf']

CLEAN.include(FileList['*.log', '*.aux', '*.dvi', '*.toc', '*.ps'])
CLOBBER.include(FileList["#{config['target']}.pdf", '**/*.bb', '**/*.xbb', '**/*.bbl', '**/*.blg'])

task :default => :open


desc "generate BoundingBox(.bb or .xbb) files"
file :bb => img_require_bb.ext('bb')


desc "generate Bibliography(.bbl & .blg) files"
file :bbl => "#{target}.bbl"

file "#{target}.bbl" => "#{target}.aux" do |t|
  sh "#{config['bbl_cmd']} #{target}"
  Rake::Task["#{target}.dvi"].execute
end


desc "compile .tex"
file :tex => "#{target}.dvi"

file "#{target}.dvi" => ["#{target}.tex", :bb, :bbl] do |t|
  sh "#{config['tex_cmd']} #{t.prerequisites.first}"
end


desc "make pdf file from .dvi"
file :dvi => "#{target}.pdf"


desc "open pdf"
task :open => "#{target}.pdf" do |t|
  sh "#{config['open_cmd']} #{t.prerequisites.first}"
end


# rules
rule '.bb' => '.png' do |t|
  sh "#{config['bb_cmd']} #{t.source}"
end

rule '.bb' => '.jpg' do |t|
  sh "#{config['bb_cmd']} #{t.source}"
end

rule '.bb' => '.pdf' do |t|
  sh "#{config['bb_cmd']} #{t.source}"
end

rule '.aux' => '.tex' do |t|
  sh "#{config['tex_cmd']} #{t.source}"
end

rule '.pdf' => '.dvi' do |t|
  sh "#{config['dvi_cmd']} #{t.source}"
end
