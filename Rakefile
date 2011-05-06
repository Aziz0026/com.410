# Author: Toksaitov Dmitrii Alexandrovich

# This is what you need to do if you don't have Excel/Calc/Numbers installed
#  on your PC/Mac

RESULT_FILE   = 'COM_410_Spring_2011_Lab_Results.pdf'
SOURCE_FILE   = 'results.yaml'
TEMPLATE_FILE = 'results.erb'
TEX_FILE      = 'results.tex'

PARSED_DATA = {}

require 'rake/clean'
CLEAN.include(TEX_FILE, '*.aux', '*.log', '*.out')
CLOBBER.include(RESULT_FILE)

desc 'Invoke the :generate task.'
task :default => :generate

desc 'Generate the resulting file.'
task :generate => RESULT_FILE

file RESULT_FILE => [TEX_FILE] do |task|
  2.times do
    sh "pdflatex #{task.prerequisites.first}"
  end
end

file TEX_FILE => [:extract_data, TEMPLATE_FILE] do |task|
  require 'erb'
  File.open(task.name, 'w+') do |io|
    io.write(ERB.new(File.read(TEMPLATE_FILE)).result)
  end
end

NUMBER_OF_TASKS    = 4
MAX_VALUE_FOR_TASK = 1.0

TOTAL_MAX  = NUMBER_OF_TASKS * MAX_VALUE_FOR_TASK
MAX_RESULT = 100.0

task :extract_data => [SOURCE_FILE] do |task|
  require 'yaml'
  PARSED_DATA = YAML.load_file(task.prerequisites.first)
  PARSED_DATA.each do |name, grades|
    grades << grades.reduce(0) do |sum, value|
      sum + value.to_f() rescue sum
    end
    grades << grades.last * MAX_RESULT / TOTAL_MAX
    grades << '-' # ToDo
  end
  PARSED_DATA = PARSED_DATA.sort { |a, b| b.last[-2] <=> a.last[-2] }
end

