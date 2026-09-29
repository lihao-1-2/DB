def scan_interfaces():
    print("\n[1/4] 扫描接口文件，提取 lives ...")
    all_lives = []
    used_urls = set()
    if not SCAN_DIR.exists():
        print(f"  目录不存在: {SCAN_DIR}")
        return all_lives
    for json_file in SCAN_DIR.glob("*.json"):
        if json_file.name == AGGREGATE_JSON.name:
            continue
        source = json_file.stem
        try:
            with open(json_file, "r", encoding="utf-8") as f:
                data = json.load(f)
        except (json.JSONDecodeError, Exception) as e:
            print(f"  跳过 {json_file.name}: {e}")
            continue
        
        # ========== 修复缩进开始 ==========
        if isinstance(data, dict):
            lives = data.get("lives", [])
        elif isinstance(data, list):
            lives = data
        else:
            lives = []
        # ========== 修复缩进结束 ==========
        
        if not isinstance(lives, list):
            continue
        valid = 0
        for item in lives:
            if not isinstance(item, dict):
                continue
            item_name = item.get("name", "").strip()
            item_url = item.get("url", "").strip()
            # 有名称且有URL，全部提取
            if not item_name or not item_url:
                continue
            if not item_url.startswith(("http://", "https://")):
                continue
            norm = normalize_url(item_url)
            if norm in used_urls:
                continue
            used_urls.add(norm)
            all_lives.append({
                "name": item_name,
                "url": item_url,
                "ua": item.get("ua", ""),
                "source": source,
            })
            valid += 1
        print(f"  {json_file.name}: {valid} 条")
    print(f"  合计（去重后）: {len(all_lives)}")
    return all_lives
