If the system is using `systemd`
### Disable

```
sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
```

### Enable
```
sudo systemctl unmask sleep.target suspend.target hibernate.target hybrid-sleep.target
```