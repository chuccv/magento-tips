# Magento and MySQL Commands Reference

## Magento Common Commands

### Cache

**Flush all cache**
```bash
php bin/magento cache:flush
```

**Clean cache**
```bash
php bin/magento cache:clean
```

**Disable cache**
```bash
php bin/magento cache:disable
```

**Enable cache**
```bash
php bin/magento cache:enable
```

**Cache status**
```bash
php bin/magento cache:status
```

---

### Indexer

**Reindex all**
```bash
php bin/magento indexer:reindex
```

**Reindex specific**
```bash
php bin/magento indexer:reindex catalog_product_price cataloginventory_stock
```

**Indexer status**
```bash
php bin/magento indexer:status
```

**Set indexer to realtime**
```bash
php bin/magento indexer:set-mode realtime
```

**Set indexer to schedule**
```bash
php bin/magento indexer:set-mode schedule
```

---

### Setup

**Setup upgrade**
```bash
php bin/magento setup:upgrade
```

**Setup upgrade (no recompile)**
```bash
php bin/magento setup:upgrade --keep-generated
```

**Static content deploy**
```bash
php bin/magento setup:static-content:deploy -f
```

**Static content deploy specific locale**
```bash
php bin/magento setup:static-content:deploy en_US vi_VN -f
```

**DI compile**
```bash
php bin/magento setup:di:compile
```

---

### Maintenance

**Enable maintenance**
```bash
php bin/magento maintenance:enable
```

**Disable maintenance**
```bash
php bin/magento maintenance:disable
```

**Maintenance status**
```bash
php bin/magento maintenance:status
```

---

### Module

**Enable module**
```bash
php bin/magento module:enable Vendor_Module
```

**Disable module**
```bash
php bin/magento module:disable Vendor_Module
```

**Disable 2FA modules**
```bash
php bin/magento module:disable Magento_AdminAdobeImsTwoFactorAuth Magento_TwoFactorAuth
```

**Module status**
```bash
php bin/magento module:status
```

---

### Cron

**Run cron**
```bash
php bin/magento cron:run
```

**Install cron**
```bash
php bin/magento cron:install
```

**Remove cron**
```bash
php bin/magento cron:remove
```

---

### Admin

**Reset admin password**
```bash
php bin/magento admin:user:unlock admin
```

**Create admin user**
```bash
php bin/magento admin:user:create \
  --admin-user=admin1 \
  --admin-password=admin123 \
  --admin-email=hi1@mageplaza.com \
  --admin-firstname=Mageplaza \
  --admin-lastname=Family
```

**Create admin token (API)**
```bash
php bin/magento admin:token:create admin
```

---

### Deploy Mode

**Set developer mode**
```bash
php bin/magento deploy:mode:set developer
```

**Set production mode**
```bash
php bin/magento deploy:mode:set production
```

**Show current mode**
```bash
php bin/magento deploy:mode:show
```

---

### Config

**Show config value**
```bash
php bin/magento config:show web/unsecure/base_url
```

**Disable admin captcha**
```bash
php bin/magento config:set admin/captcha/enable 0
```

**Disable customer captcha**
```bash
php bin/magento config:set customer/captcha/enable 0
```

**Disable form key (dev only)**
```bash
php bin/magento config:set admin/security/use_form_key 0
```

**Disable password lifetime**
```bash
php bin/magento config:set admin/security/password_lifetime 00
```

**Enable template hints**
```bash
php bin/magento dev:template-hints:enable
```

**Disable JS merge**
```bash
php -d memory_limit=-1 bin/magento config:set dev/js/merge_files 0
```

**Disable JS bundling**
```bash
php -d memory_limit=-1 bin/magento config:set dev/js/enable_js_bundling 0
```

**Enable CSS merge**
```bash
php bin/magento config:set dev/css/merge_css_files 1
```

**Disable CSS minify**
```bash
php bin/magento config:set dev/css/minify_files 0
```

**Set Elasticsearch index prefix**
```bash
php bin/magento config:set catalog/search/elasticsearch7_index_prefix ce247p3_chuccv
```

**Set base URL (ngrok)**
```bash
php bin/magento config:set web/unsecure/base_url https://open-moderately-mudfish.ngrok-free.app/DashBroard/pub/
```

---

### File Permissions

**Fix permissions**
```bash
sudo chown -R www-data:www-data generated/ var/ pub/
```

---

## Magento Install

**Install Magento**
```bash
php bin/magento setup:install \
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
```

**Install CE**
```bash
composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition=2.4.7-p3 ce247p3
```

**Install EE**
```bash
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition=2.4.7 ee247
```

**Generate fixtures (performance toolkit)**
```bash
php bin/magento setup:perf:generate-fixtures setup/performance-toolkit/profiles/ce/small.xml
```

---

## Composer

**Require module**
```bash
composer require mageplaza/module-social-login:dev-2.4-develop
```

```bash
composer require mageplaza/module-core
```

---

## Code Quality

**PHPCS check**
```bash
vendor/bin/phpcs app/code/Mageplaza/Osc/ --standard=Magento2 --severity=10 --extensions=php,phtml
```

**Generate whitelist**
```bash
php bin/magento setup:db-declaration:generate-whitelist --module-name=Mageplaza_Osc
```

---

## Xdebug

**Start with Xdebug**
```bash
XDEBUG_SESSION_START="PHPSTORM" php bin/magento se:up
```

---

## PHP Version Management

**Switch PHP version (Apache)**
```bash
sudo a2dismod php7.1
sudo a2enmod php8.1
sudo service apache2 restart
sudo update-alternatives --set php /usr/bin/php8.1
```

**Install PHP 8.2 with Magento modules**
```bash
sudo apt install php8.2 libapache2-mod-php8.2 php8.2-common php8.2-gmp php8.2-curl \
  php8.2-soap php8.2-bcmath php8.2-intl php8.2-mbstring php8.2-xmlrpc php8.2-mysql \
  php8.2-gd php8.2-xml php8.2-cli php8.2-zip
```

---

## Services Management

**Stop all services**
```bash
sudo systemctl stop apache2
sudo systemctl stop mysql
sudo systemctl stop elasticsearch
sudo systemctl stop varnish
```

**Start all services**
```bash
sudo systemctl start apache2
sudo systemctl start mysql
sudo systemctl start elasticsearch
sudo service varnish restart
```

---

## Elasticsearch

**Heap size config**
```bash
sudo nano /etc/elasticsearch/jvm.options
```

**Sample ES Query**
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

## SSH & ngrok

**Generate SSH key**
```bash
ssh-keygen -t rsa -b 4096 -C "chuccv@mageplaza.com"
```

**ngrok tunnel**
```bash
ngrok http --domain=open-moderately-mudfish.ngrok-free.app 80
```

---

## MySQL Import / Export

**Import with optimization flags**
```bash
(echo "SET SESSION unique_checks=0; SET SESSION foreign_key_checks=0; SET SESSION autocommit=0;"; gunzip -c back_up/gsp.010626.sql.gz; echo "COMMIT;") | mysql -hdb -u root -p magento
```

**Import with pv (progress bar)**
```bash
pv back_up/gsp.010626.sql.gz | gunzip | mysql -hdb -u root -p magento
```

**Export database**
```bash
mysqldump -h 127.0.0.1 -u root dashboard > dashboard.sql
```

**Import database**
```bash
mysql -u root -p database_name < dbname.sql
```

---

## Debug Snippets

**Tail MySQL log**
```bash
tail -f /var/log/mysql/mysql.log
```

**Grep in codebase**
```bash
grep -rnw "getStoreId" app/
```

**PHP debug output**
```php
echo '<pre>'; var_dump($variable); echo '</pre>';
```
