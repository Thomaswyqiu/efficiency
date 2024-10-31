import yaml
import re

def process_yaml(file_path):
    with open(file_path, 'r') as file:
        data = yaml.safe_load(file)

    for i, dep in enumerate(data['dependencies']):
        value_key = dep.get('valueKey', '')
        if value_key.endswith('=R:BID'):
            match = re.match(r'mk_(\w{3})(\w{3})(\w{3})=R:BID', value_key)
            if match:
                xxx, yyy, zzz = match.groups()
                dep['valueKey'] = f'{xxx}@{yyy}@{zzz}:bid'
                if i > 0:
                    data['dependencies'][i-1]['source'] = xxx

    with open(file_path, 'w') as file:
        yaml.dump(data, file, default_flow_style=False)

# 使用示例
process_yaml('your_file.yaml')
