
### Add ssh key
ssh-copy-id -i ~/.ssh/id_rsa.pub pure@192.168.181.8


### Install rsync on client and server

######## Команда синхронизации ##########
sudo rsync -avzhc    --exclude .DS_Store \
                --exclude 'incass_models' \
                --exclude '*.pkl' \
                --exclude '*.log' \
                --exclude '*.uds' \
                --exclude '*.out' \
                --exclude '__pycache__' \
                -e "ssh -i /home/msi/.ssh/id_rsa" \
                /home/api_source/ \
                pure@192.168.181.8:/home/api_distanation/ \
                --delete

######## Команда синхронизации ##########
Создаем саму службу как в ./systemd_services/rsync_to_jetson.service

и кладем её в
/etc/systemd/system/rsync_to_jetson.service


###### Создаем таймер ########
Создаем саму службу как в ./systemd_services/timer_rsync_jetson.timer

и кладем её в
/etc/systemd/system/timer_rsync_jetson.timer


##### Обновить службу
### copy services to /etc/systemd/
service
sudo cp /home/msi/run_service/rsync_to_jetson.service /etc/systemd/system/
timer
sudo cp /home/msi/run_service/timer_rsync_jetson.timer /etc/systemd/system/



###### Запуск  ######
systemctl daemon-reload

sudo systemctl enable rsync_to_jetson.service
sudo systemctl enable timer_rsync_jetson.timer

sudo systemctl start rsync_to_jetson.service
sudo systemctl start timer_rsync_jetson.timer


##### Выключение Disable 
sudo systemctl stop rsync_to_jetson.service
sudo systemctl stop timer_rsync_jetson.timer

sudo systemctl disable rsync_to_jetson.service
sudo systemctl disable timer_rsync_jetson.timer

systemctl status rsync_to_jetson.service
systemctl status timer_rsync_jetson.timer

## Delete
sudo rm /etc/systemd/system/rsync_to_jetson.service
sudo rm /etc/systemd/system/timer_rsync_jetson.timer


# Ensure all is well
journalctl -f -u iridium-rsync_to_jetson.service



#### Status

systemctl status rsync_to_jetson.service
systemctl status timer_rsync_jetson.timer

