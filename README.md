# mastodon_install
Instal mastodon for development (localhost)
Mở cmd trong thư mục này lên.

1. Chạy build_image.bat
2. Chạy run_container.bat
3. Chạy connect_to_container.bat

4. Cài đặt cho các package để chạy mastodon.
4.1 Setup cho Nodejs
curl -sL https://deb.nodesource.com/setup_12.x | bash -

4.2 Setup cho yarn
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

4.3 Chạy cài đặt
apt update
apt install -y \
  imagemagick ffmpeg libpq-dev libxml2-dev libxslt1-dev file git-core \
  g++ libprotobuf-dev protobuf-compiler pkg-config nodejs gcc autoconf \
  bison build-essential libssl-dev libyaml-dev libreadline6-dev \
  zlib1g-dev libncurses5-dev libffi-dev libgdbm-dev \
  nginx redis-server redis-tools postgresql postgresql-contrib \
  certbot python-certbot-nginx yarn libidn11-dev libicu-dev libjemalloc-dev

 4.4 Cài ruby
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
cd ~/.rbenv && src/configure && make -C src
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec bash
git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
RUBY_CONFIGURE_OPTS=--with-jemalloc rbenv install 2.7.2
rbenv global 2.7.2
4.5 Setup cho postgres
/etc/init.d/postgresql restart
Cài sudo:
apt-get install sudo -y
sudo -u postgres psql
CREATE USER mastodon CREATEDB;
\q
5. Cài đặt mastodon.
cd /railsapp/
git clone https://github.com/tootsuite/mastodon.git live && cd live
git checkout $(git tag -l | grep -v 'rc[0-9]*$' | sort -V | tail -n 1)
bundle install -j$(getconf _NPROCESSORS_ONLN)
yarn install --pure-lockfile
6. Chạy mastondon.
sudo -u postgres createuser -s root
export RAILS_ENV=development
bundle exec rails db:setup
bundle exec rails assets:precompile
bundle exec rails server -b 0.0.0.0 -e development

Note: Chưa setup smtp nên chưa gửi mail được => Ko đăng ký tài khoản được :'(
Tài khoản admin mặc định `admin@localhost:3000`, pass: `mastodonadmin`