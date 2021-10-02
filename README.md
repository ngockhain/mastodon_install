# mastodon_install
Instal mastodon for development (localhost)

Vao quyen admin
su #Nhap password

Setup Nodejs
curl -sL https://deb.nodesource.com/setup_12.x | sudo bash -

Setup cho Yarn
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - 
echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

Chạy cài đặt
apt update
apt install -y imagemagick ffmpeg libpq-dev libxml2-dev libxslt1-dev file git-core g++ libprotobuf-dev protobuf-compiler pkg-config nodejs gcc autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm-dev nginx redis-server redis-tools postgresql postgresql-contrib certbot python-certbot-nginx yarn libidn11-dev libicu-dev libjemalloc-dev

Tạo user mastodon
(Tao nguoi dung de neu sau nay ko xai nua thi chi can xoa nguoi dung thoi)
adduser mastodon

Chuyển người dùng
su - mastodon

Cài ruby
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
cd ~/.rbenv && src/configure && make -C src
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec bash
git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
RUBY_CONFIGURE_OPTS=--with-jemalloc rbenv install 2.7.2
rbenv global 2.7.2

Setup cho postgres
exit # Trả về root user
sudo -u postgres createuser -s root
systemctl restart postgresql
sudo -u postgres psql
CREATE USER mastodon CREATEDB;
\q

Cài đặt mastodon.
su - mastodon
git clone https://github.com/tootsuite/mastodon.git live && cd live
git checkout $(git tag -l | grep -v 'rc[0-9]*$' | sort -V | tail -n 1)
bundle install -j$(getconf _NPROCESSORS_ONLN)
yarn install --pure-lockfile

Chạy mastondon.
bundle exec rails db:setup
bundle exec rails assets:precompile

export RAILS_ENV=development && bundle exec rails server
