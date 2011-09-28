require 'rake/clean'
require 'yaml'

config = YAML.load_file(File.dirname(__FILE__) + '/config.yaml')
img_require_bb = FileList['img/*.png', 'img/*.jpg', 'img/*.pdf']

CLEAN.include(FileList['*.log', '*.aux', '*.dvi', '*.toc', '*.ps'])
CLOBBER.include(FileList['*.pdf', '**/*.bb', '**/*.xbb'])

task :pdf => :dvi
task :default => :open

desc "generate BoundingBox(.bb or .xbb) files inside 'img' directory'"
file :bb => img_require_bb.ext('bb')


desc "compile .tex"
file :tex => "#{config['target']}.dvi"

file "#{config['target']}.dvi" => ["#{config['target']}.tex", :bb] do |t|
  sh "#{config['tex_cmd']} #{t.prerequisites.first}"
end


desc "make pdf file from .dvi"
file :dvi => "#{config['target']}.pdf"

file "#{config['target']}.pdf" => "#{config['target']}.dvi" do |t|
  sh "#{config['dvi_cmd']} #{t.prerequisites.first}"
end


desc "open pdf"
task :open => "#{config['target']}.pdf" do |t|
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
