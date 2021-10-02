# mastodon_install
Instal mastodon for development (localhost)

1. Vao quyen admin
> su #Nhap password

2. Setup Nodejs
> curl -sL https://deb.nodesource.com/setup_12.x | sudo bash -

3. Setup cho Yarn
> curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - <br>
> echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

4. Chạy cài đặt
> apt update <br>
> apt install -y imagemagick ffmpeg libpq-dev libxml2-dev libxslt1-dev file git-core g++ libprotobuf-dev protobuf-compiler pkg-config nodejs gcc autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm-dev nginx redis-server redis-tools postgresql postgresql-contrib certbot python-certbot-nginx yarn libidn11-dev libicu-dev libjemalloc-dev

5. Tạo user mastodon
(Tạo người dùng để nếu sau này ko xài nữa thì chỉ cần xóa người dùng thôi)
> adduser mastodon
6. Chuyển người dùng
> su - mastodon

7. Cài ruby
> git clone https://github.com/rbenv/rbenv.git ~/.rbenv <br>
> cd ~/.rbenv && src/configure && make -C src <br>
> echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc <br>
> echo 'eval "$(rbenv init -)"' >> ~/.bashrc <br>
> exec bash <br>
> git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build <br>
> RUBY_CONFIGURE_OPTS=--with-jemalloc rbenv install 2.7.2 <br>
> rbenv global 2.7.2

8. Setup cho postgres
> exit # Trả về root user <br>
> sudo -u postgres createuser -s root <br>
> systemctl restart postgresql <br>
> sudo -u postgres psql <br>
> CREATE USER mastodon CREATEDB; <br>
> \q

9. Cài đặt mastodon
> su - mastodon <br>
> git clone https://github.com/tootsuite/mastodon.git live && cd live <br>
> git checkout $(git tag -l | grep -v 'rc[0-9]*$' | sort -V | tail -n 1) <br>
> bundle install -j$(getconf _NPROCESSORS_ONLN) <br>
> yarn install --pure-lockfile

10. Chạy mastondon.
> bundle exec rails db:setup <br>
> bundle exec rails assets:precompile <br>
> export RAILS_ENV=development && bundle exec rails server
