# grep -oE "cassette(.*)" spec/requests/**/* | sed s/\'/\"/g | grep -o '".*"' | sed 's/"//g' > used_cassettes
# find spec/fixtures/cassettes/**/*.yml -type f > cassettes

#used_cassettes = File.read('used_cassettes').split("\n").map{|c| "spec/fixtures/cassettes/#{c}.yml" }
#cassettes      = File.read('cassettes').split("\n")

# find spec/fixtures/cassettes/**/* -type d -empty -exec rmdir {} \;
# find spec/fixtures/cassettes/**/* -type d -empty -delete

class Cassettes
  PREFIX_CASSETTES = 'spec/fixtures/cassettes/'

  def self.all_files
    Dir.glob("#{PREFIX_CASSETTES}**/*.yml")
  end

  def self.all_called
    Dir.glob("spec/**/*.rb").inject([]) do |cassettes_names, file|
      File.read(file).grep(/cassette/).each do |list|
        cassettes_names << "#{PREFIX_CASSETTES}#{list[/(['|"]([^,]+)['|"])/, 2]}.yml"
      end
      cassettes_names
    end.uniq
  end

  def self.diferences
    self.all_files - self.all_called
  end
end

namespace :VCR do
  desc 'Total of diferences'
  task :total do
    all_called = Cassettes.all_called.count
    all_files  = Cassettes.all_files.count
    puts "used_cassettes \t= #{all_called}"
    puts "cassettes files\t= #{all_files}"
    puts "TOTAL          \t= #{all_files - all_called}"
  end

  desc 'Show files diferences'
  task :diferences do
    puts Cassettes.diferences
  end

  desc 'Write all diferences on log (not_used.log)'
  task :logs do
    f = File.new('not_used.log', 'w+')
    Cassettes.diferences.each {|cassette| f.write("#{cassette}\n") }
    f.close
    puts 'Log writed.'
  end

  namespace :delete do
    desc 'Delete files and folders not used (not reversible)'
    task :all do
      #TODO remove cassettes not used and folders
      #Cassettes.diferences.each {|cassette| File.delete(cassette) }
    end

    desc 'Delete files and folders not used (names on log file)'
    task :of_file do
      #TODO remove cassettes not used and folders
    end

    desc 'Delete files and folders not used (backup on /tmp/to_delete_cassettes/)'
    task :with_backup do
      #TODO remove cassettes not used and folders with bkp
    end
  end
end
