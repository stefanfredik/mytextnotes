# Docker

### Error Saat Kongfigurasi Docker :&#x20;

```bash
Cannot connect to unix socket /var/run/docker.sock ssl:default [Permission denied]
```

Cara Mengatasi

Cara 1

<pre class="language-bash"><code class="lang-bash"><strong>id -u your_username
</strong>sudo usermod -aG docker your_username
</code></pre>

Cara 2&#x20;

```bash
sudo chown your_username:your_group /var/run/docker.sock
sudo chmod 660 /var/run/docker.sock
```
