#!/bin/bash

# 定义环境变量存储目录

UVENV_program='uvenv'
UVENV_ENV_DIR="${HOME}/.$UVENV_program"
UVENV_CURRENT=""


# 帮助函数
_uv_usage() {
    echo "用法: $UVENV_program <命令> [选项]"
    echo "命令:"
    echo "   list        列出所有环境"
    echo "   create      创建一个新环境"
    echo "   delete      删除一个环境 (del)"
    echo "   activate    激活一个环境 (act)"
    echo "   deactivate  退出当前环境 (deact)"
    echo "   help        显示此帮助信息"
    echo "   install     安装命令"
    echo "选项:"
    echo "   name      环境名称 (create 和 delete 命令需要)"
}

# 列出所有环境
_uv_list_envs() {
    if [ ! -d "$UVENV_ENV_DIR" ]; then
        echo "没有已创建的环境."
        return 0
    fi
    echo "已创建的环境:"
    find "$UVENV_ENV_DIR" -maxdepth 1 -type d -print0 | while IFS= read -r -d $'\0' dir; do
        # 提取目录名
        env_name=$(basename "$dir")
        if [ "$env_name" != ".$UVENV_program" ]; then # 排除根目录
            echo "  $env_name"
        fi
    done
}

# 创建新环境
_uv_create_env() {
    if [ -z "$2" ]; then
        echo "错误: 必须指定环境名称."
        return 1
    fi
    local env_name="$2"
    local env_path="$UVENV_ENV_DIR/$env_name"
    if [ -d "$env_path" ]; then
        echo "环境 '$env_name' 已经存在."
        return 1
    fi
    # mkdir -p "$env_path"
    # 移除前面两个参数 create 和环境名称
    shift 2
    uv venv "$env_path" $@
    if [ $? -eq 0 ]; then
        echo "环境 '$env_name' 创建成功，位于 $env_path."
    else
        echo "创建环境 '$env_name' 失败."
        return 1
    fi
}

# 删除环境
_uv_delete_env() {
    if [ -z "$2" ]; then
        echo "错误: 必须指定要删除的环境名称."
        return 1
    fi
    local env_name="$2"
    local env_path="$UVENV_ENV_DIR/$env_name"
    if [ ! -d "$env_path" ]; then
        echo "环境 '$env_name' 不存在."
        return 1
    fi
    rm -rf "$env_path"
    if [ $? -eq 0 ]; then
        echo "环境 '$env_name' 删除成功."
    else
        echo "删除环境 '$env_name' 失败."
        return 1
    fi
}

_uv_activate_env() {
    if [ -z "$2" ]; then
        echo "错误: 必须指定要激活的环境名称."
        return 1
    fi
    local env_name="$2"
    local env_path="$UVENV_ENV_DIR/$env_name"
    if [ ! -d "$env_path" ]; then
        echo "环境 '$env_name' 不存在."
        return 1
    fi
    source "$env_path/bin/activate"
    UVENV_CURRENT="$env_path/bin/activate"
}

_uv_deactivate_env() {
    deactivate
    UVENV_CURRENT=""
    # 清除当前环境变量
}

uvenv() {
    # 主程序
    if [ -z "$1" ]; then
        _uv_usage
        return 1
    fi

    if [ ! -d "$UVENV_ENV_DIR" ]; then
        # 确保环境变量目录存在
        # echo "UVENV_ENV_DIR: $UVENV_ENV_DIR"
        mkdir -p "$UVENV_ENV_DIR"
    fi

    case "$1" in
    list)
        _uv_list_envs
        ;;
    create)
        _uv_create_env "$@"
        ;;
    delete)
        _uv_delete_env "$@"
        ;;
    del)
        _uv_delete_env "$@"
        ;;
    activate)
        _uv_activate_env "$@"
        ;;
    act)
        _uv_activate_env "$@"
        ;;
    deactivate)
        _uv_deactivate_env "$@"
        ;;
    deact)
        _uv_deactivate_env "$@"
        ;;
    help)
        _uv_usage
        ;;
    *)
        echo "错误: 未知的命令 '$1'."
        _uv_usage
        return 1
        ;;
    esac
}


if [ ! -z "$UVENV_CURRENT" ]; then
    source $UVENV_CURRENT
fi