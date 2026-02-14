# Deploy Skill

Complex multi-step workflow for deploying Tinker to the Raspberry Pi.

## Steps
1. Run full test suite: `pytest src/ -v`
2. Lint: `ruff check src/`
3. Sync files to Pi: `rsync -avz --exclude '.venv' --exclude 'models/' --exclude '__pycache__' . pi@tinker.local:~/tinker/`
4. SSH into Pi and install dependencies: `ssh pi@tinker.local 'cd ~/tinker && pip install -e .'`
5. Restart the service: `ssh pi@tinker.local 'sudo systemctl restart tinker'`
6. Verify service is running: `ssh pi@tinker.local 'systemctl status tinker'`

## Tool Permissions Needed
- Bash (rsync, ssh, systemctl)
- Read (test results, service status)
