# MySQL & Magento Commands Reference

## MySQL Import / Export

```bash
# Import with optimization flags
(echo "SET SESSION unique_checks=0; SET SESSION foreign_key_checks=0; SET SESSION autocommit=0;"; gunzip -c back_up/gsp.010626.sql.gz; echo "COMMIT;") | mysql -hdb -u root -p magento

# Import with pv (progress bar)
pv back_up/gsp.010626.sql.gz | gunzip | mysql -hdb -u root -p magento

# Export database
mysqldump -h 127.0.0.1 -u root dashboard > dashboard.sql
# mysqldump -h host_name -u username database_name tableName > tableName.sql

# Import database
mysql -u root -p database_name < dbname.sql
# mysql -p -u username database_name < file.sql
```

---

## Magento CLI Commands

### Setup & Install

```bash
# Install Magento
sudo php bin/magento setup:install \
  --base-url=http://ce247.com/ \
  --db-host=127.0.0.1 \
  --db-name=ce247 \
  --db-user=root \
  --db-password=vanchuc97 \
  --admin-firstname=admin \
  --admin-lastname=admin \
  --admin-email=admin@admin.com \
  --admin-user=admin \
  --admin-password=admin123 \
  --language=en_US \
  --currency=USD \
  --timezone=America/Chicago \
  --use-rewrites=1 \
  --backend-frontname="admin" \
  --search-engine=elasticsearch7 \
  --cleanup-database

# Create admin user
php bin/magento admin:user:create \
  --admin-user=admin1 \
  --admin-password=admin123 \
  --admin-email=hi1@mageplaza.com \
  --admin-firstname=Mageplaza \
  --admin-lastname=Family

# Composer create project
composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition=2.4.7-p3 ce247p3
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition=2.4.7 ee247
```

### Module Management

```bash
# Disable 2FA modules
sudo php bin/magento module:disable Magento_AdminAdobeImsTwoFactorAuth Magento_TwoFactorAuth

# DI compile
php -d memory_limit=5G bin/magento setup:di:compile

# Set developer mode
php bin/magento deploy:mode:set developer

# Generate fixtures (performance toolkit)
php bin/magento setup:perf:generate-fixtures setup/performance-toolkit/profiles/ce/small.xml
```

### Config Settings

```bash
# Disable captcha
sudo php bin/magento config:set admin/captcha/enable 0
sudo php bin/magento config:set customer/captcha/enable 0

# Disable form key (dev only)
sudo php bin/magento config:set admin/security/use_form_key 0

# Disable password lifetime
php bin/magento config:set admin/security/password_lifetime 00

# Template hints
sudo php bin/magento dev:template-hints:enable

# JS/CSS optimization
php -d memory_limit=-1 bin/magento config:set dev/js/merge_files 0
php -d memory_limit=-1 bin/magento config:set dev/js/enable_js_bundling 0
php bin/magento config:set dev/css/merge_css_files 1
php bin/magento config:set dev/css/minify_files 0

# Elasticsearch index prefix
php bin/magento config:set catalog/search/elasticsearch7_index_prefix ce247p3_chuccv

# Base URL (ngrok)
sudo bin/magento config:set web/unsecure/base_url https://open-moderately-mudfish.ngrok-free.app/DashBroard/pub/
```

### File Permissions

```bash
sudo chown -R www-data:www-data generated/ var/ pub/
```

---

## PHP Version Management

```bash
# Switch PHP version (Apache)
sudo a2dismod php7.1
sudo a2enmod php8.1
sudo service apache2 restart
sudo update-alternatives --set php /usr/bin/php8.1

# Install PHP 8.2 with Magento modules
sudo apt install php8.2 libapache2-mod-php8.2 php8.2-common php8.2-gmp php8.2-curl \
  php8.2-soap php8.2-bcmath php8.2-intl php8.2-mbstring php8.2-xmlrpc php8.2-mysql \
  php8.2-gd php8.2-xml php8.2-cli php8.2-zip
```

---

## Services Management

```bash
# Stop all services
sudo systemctl stop apache2
sudo systemctl stop mysql
sudo systemctl stop elasticsearch
sudo systemctl stop varnish

# Start all services
sudo systemctl start apache2
sudo systemctl start mysql
sudo systemctl start elasticsearch
sudo service varnish restart
```

---

## Elasticsearch

```bash
# Heap size config
sudo nano /etc/elasticsearch/jvm.options
```

### Sample ES Query

```json
{
  "from": 0,
  "size": 100,
  "_source": ["sku", "discount_0"],
  "docvalue_fields": ["_id", "_score"],
  "sort": [{ "discount_0": { "order": "asc" } }],
  "query": {
    "bool": {
      "must": [
        { "term": { "category_ids": "4" } },
        { "terms": { "visibility": ["2", "4"] } }
      ]
    }
  }
}
```

---

## Code Quality

```bash
# PHPCS check
vendor/bin/phpcs app/code/Mageplaza/Osc/ --standard=Magento2 --severity=10 --extensions=php,phtml

# Generate whitelist
sudo php bin/magento setup:db-declaration:generate-whitelist --module-name=Mageplaza_Osc

# Coding standard reference
# https://github.com/magento/magento-coding-standard
```

---

## Composer

```bash
composer require mageplaza/module-social-login:dev-2.4-develop
composer require mageplaza/module-core
```

---

## Xdebug

```bash
XDEBUG_SESSION_START="PHPSTORM" php bin/magento se:up
```

---

## SSH & ngrok

```bash
# Generate SSH key
ssh-keygen -t rsa -b 4096 -C "chuccv@mageplaza.com"

# ngrok tunnel
ngrok http --domain=open-moderately-mudfish.ngrok-free.app 80
```

---

## Useful Debug Snippets

```bash
# Tail MySQL log
sudo tail -f /var/log/mysql/mysql.log

# Grep in codebase
grep -rnw "getStoreId" app/
grep "getHtmlSlider"
```

```php
echo '<pre>'; var_dump($variable); echo '</pre>';
```

---

## Quick Scripts

```bash
# Install script shortcut
sh install.sh ce243 chuccv ce243
```
