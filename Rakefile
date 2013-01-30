user = "martin.harrigan"
host = "socmedvis.swansea.ac.uk"
directory = "/srv/www/htdocs-socmedvis"

task :parse_haml => [:parse_haml_pages] do
  system(%{
    cd _source/_layouts/haml && for f in *.haml; 
    do [ -e $f ] && haml $f ../${f%.haml}.html; done
  })
end

task :parse_haml_pages => [:parse_haml_posts] do
  system(%{
    cd _source/_layouts/pages && for f in *.haml; 
    do [ -e $f ] && haml $f ../../${f%.haml}.html; done
  })
end

task :parse_haml_posts => [:parse_sass] do
  system(%{
    cd _source/_layouts/posts && for f in *.haml; 
    do [ -e $f ] && haml $f ../../_posts/${f%.haml}.html; done
  })
end

task :parse_sass do
  system(%{
    cd _source/_layouts/sass && for f in *.scss; 
    do [ -e $f ] && sass $f ../../css/${f%.scss}.css; done
  })
end

task :preview do
  Rake::Task["parse_haml"].invoke
  system "jekyll --auto --server"
end

task :build do |task, args|
  Rake::Task["parse_haml"].invoke
  system "jekyll"
end

desc "Grabs the public site and sends it up to your server."
task :deploy do
  system("rsync -avz --delete _public/ #{user}@#{host}:#{directory}")
end
