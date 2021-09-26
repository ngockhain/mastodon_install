# mastodon_install
### - For docker users
Instal mastodon for development (localhost)
Mở cmd trong thư mục này lên.

- Tạo thư mục mới, đặt tên là railsapp. Thư mục này sẽ được clone code của mastodon (trong bước 5).
1. Chạy build_image.bat
2. Chạy run_container.bat
3. Chạy connect_to_container.bat

4. Cài đặt cho các package để chạy mastodon.
  - Setup cho Nodejs
> curl -sL https://deb.nodesource.com/setup_12.x | bash -

  - Setup cho yarn
> curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
> echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

  - Chạy cài đặt
> apt update

> apt install -y \
  imagemagick ffmpeg libpq-dev libxml2-dev libxslt1-dev file git-core \
  g++ libprotobuf-dev protobuf-compiler pkg-config nodejs gcc autoconf \
  bison build-essential libssl-dev libyaml-dev libreadline6-dev \
  zlib1g-dev libncurses5-dev libffi-dev libgdbm-dev \
  nginx redis-server redis-tools postgresql postgresql-contrib \
  certbot python-certbot-nginx yarn libidn11-dev libicu-dev libjemalloc-dev

  - Cài ruby
> git clone https://github.com/rbenv/rbenv.git ~/.rbenv<br>
> cd ~/.rbenv && src/configure && make -C src<br>
> echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc<br>
> echo 'eval "$(rbenv init -)"' >> ~/.bashrc<br>
> exec bash<br>
> git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build<br>
> RUBY_CONFIGURE_OPTS=--with-jemalloc rbenv install 2.7.2<br>
> rbenv global 2.7.2
  - Setup cho postgres
> /etc/init.d/postgresql restart
Cài sudo:
> apt-get install sudo -y <br>
> sudo -u postgres psql <br>
> CREATE USER mastodon CREATEDB; <br>
> \q
5. Cài đặt mastodon.
> cd /railsapp/ <br>
> git clone https://github.com/tootsuite/mastodon.git live && cd live <br>
> git checkout $(git tag -l | grep -v 'rc[0-9]*$' | sort -V | tail -n 1) <br>
> bundle install -j$(getconf _NPROCESSORS_ONLN) <br>
> yarn install --pure-lockfile <br>
6. Chạy mastondon.
> sudo -u postgres createuser -s root <br>
> export RAILS_ENV=development <br>
> bundle exec rails db:setup <br>
> bundle exec rails assets:precompile <br>
> bundle exec rails server -b 0.0.0.0 -e development <br>

### - For ubuntu users (Ubuntu 18.04)
- Vào quyền root
> su
1. Setup cho Nodejs
> curl -sL https://deb.nodesource.com/setup_12.x | bash -

2. Setup cho yarn
> curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
> echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

3. Chạy cài đặt
> apt update

> apt install -y \
  imagemagick ffmpeg libpq-dev libxml2-dev libxslt1-dev file git-core \
  g++ libprotobuf-dev protobuf-compiler pkg-config nodejs gcc autoconf \
  bison build-essential libssl-dev libyaml-dev libreadline6-dev \
  zlib1g-dev libncurses5-dev libffi-dev libgdbm-dev \
  nginx redis-server redis-tools postgresql postgresql-contrib \
  certbot python-certbot-nginx yarn libidn11-dev libicu-dev libjemalloc-dev

4. Tạo user mastodon
> adduser --disabled-login mastodon
- Chuyển người dùng
> su - mastodon
5. Cài ruby
> git clone https://github.com/rbenv/rbenv.git ~/.rbenv<br>
> cd ~/.rbenv && src/configure && make -C src<br>
> echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc<br>
> echo 'eval "$(rbenv init -)"' >> ~/.bashrc<br>
> exec bash<br>
> git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build<br>
> RUBY_CONFIGURE_OPTS=--with-jemalloc rbenv install 2.7.2<br>
> rbenv global 2.7.2
6. Setup cho postgres
> exit # Trả về root user<br>
> sudo -u postgres createuser -s root <br>
> systemctl restart postgresql<br>
> sudo -u postgres psql <br>
> CREATE USER mastodon CREATEDB; <br>
> \q
7. Cài đặt mastodon.
> su - mastodon <br>
> cd /railsapp/ <br>
> git clone https://github.com/tootsuite/mastodon.git live && cd live <br>
> git checkout $(git tag -l | grep -v 'rc[0-9]*$' | sort -V | tail -n 1) <br>
> bundle install -j$(getconf _NPROCESSORS_ONLN) <br>
> yarn install --pure-lockfile <br>
8. Chạy mastondon.
> export RAILS_ENV=development <br>
> bundle exec rails db:setup <br>
> bundle exec rails assets:precompile <br>
> bundle exec rails server -e development <br>

Note: Chưa setup smtp nên chưa gửi mail được => Ko đăng ký tài khoản được :'( <br>
Tài khoản admin mặc định `admin@localhost:3000`, pass: `mastodonadmin`
