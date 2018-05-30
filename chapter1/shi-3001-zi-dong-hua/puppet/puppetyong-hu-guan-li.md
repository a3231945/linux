###一、用户管理
- **生成用户秘钥：**

    grub-crypt --sha-512
    Password: 
    Retype password: 
    $6$lYIpu8h4TH7Nf0RM$3lUprHO3gy4XqmCwf0EMqyZjJz1alnISVEk0D/VV9pR3jVBhrzk/vysBRWaTfAZjiuYmc/OrPas8Hs8torqm91
- **普通创建**

    vim /etc/puppet/manifests/site.pp
    user { 'longge':
        ensure     => present,
        home       => '/home/longge',
        managehome => true,
        password   => '$6$lYIpu8h4TH7Nf0RM$3lUprHO3gy4XqmCwf0EMqyZjJz1alnISVEk0D/VV9pR3jVBhrzk/vysBRWaTfAZjiuYmc/OrPas8Hs8torqm91',
    }
    
- **指定UID 制定GID指定组**

    group { 'longge':
        ensure => present,
        gid    => 1000,
    }
    user { 'longge':
        ensure     => present,
        home       => '/home/longge',
        managehome => true,
        uid        => 1000,
        gid        => 1000,
        groups     => ['longge', 'wheel'],
        password   => '$6$lYIpu8h4TH7Nf0RM$3lUprHO3gy4XqmCwf0EMqyZjJz1alnISVEk0D/VV9pR3jVBhrzk/vysBRWaTfAZjiuYmc/OrPas8Hs8torqm91',
    }
- **删除用户**

    user { 'longge':
            ensure => absent,
            home   => '/home/longge',
            managehome => true,
    }
###二、用户组管理
- **创建一个组**

    vim /etc/puppet/manifests/site.pp
    group { 'longge': ensure => present }
- **创建一个组，指定GID**

    vi /etc/puppet/manifests/site.pp
    group { 'longge':
        ensure => present,
        gid    => 1000,
    }
- **删除一个组**

    vim /etc/puppet/manifests/site.pp
    group { 'longge': ensure => absent }
